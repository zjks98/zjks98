### 2022.6.15  星期三

在服务器上实验一篇有关pg优化器成本因子校对的实验，数据集为imdb，对象为其中的四个表格。

movie_companies，2609129行；

movie_info，14835720行；

movie_keyword, 4523930行；

title, 2528312行；

几个路径：

/usr/local/pgsql/bin/postgres

/usr/local/pgsql/data/postgresql.conf



## 1.建表：

```
imdb3=# CREATE TABLE movie_companies ( 

imdb3(#     id integer NOT NULL PRIMARY KEY,  

imdb3(#     movie_id integer NOT NULL,  

imdb3(#     company_id integer NOT NULL,  

imdb3(#     company_type_id integer NOT NULL,  

imdb3(#     note text  

imdb3(# );  

CREATE TABLE

imdb3=# CREATE TABLE movie_info (

imdb3(#     id integer NOT NULL PRIMARY KEY,

imdb3(#     movie_id integer NOT NULL,

imdb3(#     info_type_id integer NOT NULL,

imdb3(#     info text NOT NULL,

imdb3(#     note text

imdb3(# );

CREATE TABLE

imdb3=# CREATE TABLE movie_keyword (

imdb3(#     id integer NOT NULL PRIMARY KEY,

imdb3(#     movie_id integer NOT NULL,

imdb3(#     keyword_id integer NOT NULL

imdb3(# );

CREATE TABLE

imdb3=# CREATE TABLE title (

imdb3(#     id integer NOT NULL PRIMARY KEY,

imdb3(#     title text NOT NULL,

imdb3(#     imdb_index character varying(12),

imdb3(#     kind_id integer NOT NULL,

imdb3(#     production_year integer,

imdb3(#     imdb_id integer,

imdb3(#     phonetic_code character varying(5),

imdb3(#     episode_of_id integer,

imdb3(#     season_nr integer,

imdb3(#     episode_nr integer,

imdb3(#     series_years character varying(49),

imdb3(#     md5sum character varying(32)

imdb3(# );

```

## 2.导入数据

```
imdb3=# Copy movie_companies FROM '/usr/imdb/movie_companies1.csv' WITH DELIMITER AS '|' NULL '';

COPY 2609129

imdb3=# Copy movie_info FROM '/usr/imdb/movie_info1.csv' WITH DELIMITER AS '|' NULL '';

COPY 14835720

imdb3=# Copy movie_keyword FROM '/usr/imdb/movie_keyword1.csv' WITH DELIMITER AS '|' NULL '';

ERROR:  could not open file "/usr/imdb/movie_keyword1.csv" for reading: 没有那个文件或目录

HINT:  COPY FROM instructs the PostgreSQL server process to read a file. You may want a client-side facility such as psql's \copy.

imdb3=# Copy movie_keyword FROM '/usr/imdb/movie_keyword.csv' WITH DELIMITER AS ',' NULL '';

COPY 4523930

imdb3=# Copy title FROM '/usr/imdb/title1.csv' WITH DELIMITER AS '|' NULL '';

COPY 2528312

```

以上完成了数据库和数据集的准备，下面实现对我们的服务器上的postgres中估算代价时的代价因子进行修正。

修正原理：PG在估算cost时，会利用一些代价因子带入公式计算，这些因子包含，cpu_tuple_cost = 0.01,seq_page_cost = 1,cpu_operator_cost = 0.0025等

分别表示，CPU处理一个元组的cost，连续块扫描操作的单个块的cost，CPU处理一个操作（谓词过滤等）时的cost，等

这些都是PG预先设计好的定值，但是实际情况中，这些值并不是如此，想要修正它们，需要根据SQL语句实际执行的时间来修正

我们知道，优化器之所以对于一个执行计划计算出一个cost来，就是为了比较在同一个SQL语句下，不同执行计划的优劣，而这个优劣如果放到实际执行时，可以判定为该计划的实际执行时间

那么如果我们计算的cost就是实际的执行时间，或者很接近实际执行时间，那么执行计划的final_cost，比较起来就更有说服力。


## 3. 推算seq_page_cost和cpu_tuple_cost

首先分析几个表格，产生它们的统计信息

例如：

```
imdb3=# select pg_backend_pid();

 pg_backend_pid
 
----------------

         133424
				 
(1 row)

imdb3=# analyze movie_companies;

ANALYZE

imdb3=# select relpages from pg_class where relname = 'movie_companies';

 relpages
 
----------

    18790
		
(1 row)

```

其中，不同表所占的页数为：

title : 35998;

movie_companies : 18790;

movie_info : 161993;

movie_keyword : 24454;


执行checkpoint

```
imdb3=# checkpoint;

CHECKPOINT

```

退出数据库，关闭数据库
```
imdb3=#\q

[postgres@oracledb ~]$ pg_ctl stop -m fast

waiting for server to shut down..... done

server stopped

```

切换root用户：su

防止操作系统Cache的干扰, 所以要清理操作系统cache：

```
(base) [root@oracledb postgres]# sync; echo 3 > /proc/sys/vm/drop_caches

```

利用systemtap来获得每次IO请求的实际时间：

为了避免systemtap本身运行的开销，导致IO的请求时间估算的不准确，通过设置CPU亲和来减少这种影响

将被跟踪进程的亲和与stap运行的进程亲和分开，设置不同的CPU亲和，就是使用不同的CPU内核来实现该进程，Linux进程使用哪个CPU资源是由内核进行调度的。

回到postgres用户

```
(base) [root@oracledb postgres]# su - postgres

上一次登录：三 6月 15 10:04:30 CST 2022pts/1 上

[postgres@oracledb ~]$

```

指定亲和1, 启动数据库 :

```
(base) [root@oracledb postgres]# taskset -c 1 service postgresql start

Starting PostgreSQL: ok

```

开启psql，进入数据库：

```
(base) [root@oracledb postgres]# su - postgres

上一次登录：三 6月 15 16:28:16 CST 2022pts/0 上

[postgres@oracledb ~]$ psql

Password for user postgres:

psql (12.1)

Type "help" for help.

postgres=# \c imdb3

You are now connected to database "imdb3" as user "postgres".

imdb3=#

```

```
imdb3=# select pg_backend_pid();

 pg_backend_pid
 
----------------

         252792
				 
(1 row)
```

(先安装了systemtap）

```
sudo yum install systemtap
```

安装完systemtap后，使用时碰到了两次错误：

```
Checking "/lib/modules/3.10.0-1160.el7.x86_64/build/.config" failed with error: 没有那个文件或目录

Incorrect version or missing kernel-devel package, use: yum install kernel-devel-3.10.0-1160.el7.x86_64

```

```
semantic error: no match

semantic error: while resolving probe point: identifier 'kernel' at /usr/share/systemtap/tapset/linux/vfs.stp:991:25

        source: probe vfs.read.return = kernel.function("vfs_read").return
                                        ^

semantic error: missing x86_64 kernel/module debuginfo [man warning::debuginfo] under '/lib/modules/3.10.0-1160.el7.x86_64/build'

Missing separate debuginfos, use: debuginfo-install kernel-3.10.0-1160.el7.x86_64 kmod-kvdo-6.1.3.23-5.el7.x86_64

Pass 2: analysis failed.  [man error::pass2]

Number of similar error messages suppressed: 2.

Rerun with -v to see them.

```

以上两个错误，暂时不是很理解是何种含义，但是按照其指示，利用yum安装好缺少的内容

遇到了一个错误：

```
Pass 1: parsed user script and 477 library scripts using 277536virt/76512res/3752shr/72980data kb, in 560usr/30sys/588real ms.

semantic error: resolution failed in DWARF builder

semantic error: while resolving probe point: identifier 'process' at <input>:3:7

        source: probe process("/usr/local/pgsql/bin/postgres").mark("query__start") {
                      ^

semantic error: no match

semantic error: resolution failed in DWARF builder

semantic error: while resolving probe point: identifier 'process' at :12:7

        source: probe process("/usr/local/pgsql/bin/postgres").mark("query__done") {
                      ^

semantic error: no match

Pass 2: analyzed script: 2 probes, 380 functions, 12 embeds, 19 globals using 446772virt/242368res/5084shr/242216data kb, in 1250usr/400sys/1652real ms.

Pass 2: analysis failed.  [man error::pass2]

```
