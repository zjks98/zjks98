create table：
CREATE TABLE NATION  ( N_NATIONKEY  INTEGER NOT NULL,N_NAME CHAR(25) NOT NULL,N_REGIONKEY  INTEGER NOT NULL,N_COMMENT    VARCHAR(152));
CREATE TABLE REGION  ( R_REGIONKEY  INTEGER NOT NULL,R_NAME       CHAR(25) NOT NULL,R_COMMENT    VARCHAR(152));
CREATE TABLE PART  ( P_PARTKEY     INTEGER NOT NULL, P_NAME        VARCHAR(55) NOT NULL,P_MFGR        CHAR(25) NOT NULL,P_BRAND       CHAR(10) NOT NULL,P_TYPE        VARCHAR(25) NOT NULL, P_SIZE        INTEGER NOT NULL,P_CONTAINER   CHAR(10) NOT NULL,P_RETAILPRICE DECIMAL(15,2) NOT NULL, P_COMMENT     VARCHAR(23) NOT NULL );
CREATE TABLE SUPPLIER ( S_SUPPKEY     INTEGER NOT NULL,S_NAME        CHAR(25) NOT NULL, S_ADDRESS     VARCHAR(40) NOT NULL, S_NATIONKEY   INTEGER NOT NULL,S_PHONE       CHAR(15) NOT NULL, S_ACCTBAL     DECIMAL(15,2) NOT NULL,S_COMMENT     VARCHAR(101) NOT NULL);
CREATE TABLE PARTSUPP ( PS_PARTKEY     INTEGER NOT NULL, PS_SUPPKEY     INTEGER NOT NULL, PS_AVAILQTY    INTEGER NOT NULL,PS_SUPPLYCOST  DECIMAL(15,2)  NOT NULL, PS_COMMENT     VARCHAR(199) NOT NULL );
CREATE TABLE CUSTOMER ( C_CUSTKEY     INTEGER NOT NULL, C_NAME        VARCHAR(25) NOT NULL,C_ADDRESS     VARCHAR(40) NOT NULL, C_NATIONKEY   INTEGER NOT NULL, C_PHONE       CHAR(15) NOT NULL, C_ACCTBAL     DECIMAL(15,2)   NOT NULL, C_MKTSEGMENT  CHAR(10) NOT NULL, C_COMMENT     VARCHAR(117) NOT NULL);
CREATE TABLE ORDERS  ( O_ORDERKEY       INTEGER NOT NULL,O_CUSTKEY        INTEGER NOT NULL, O_ORDERSTATUS    CHAR(1) NOT NULL,O_TOTALPRICE     DECIMAL(15,2) NOT NULL,O_ORDERDATE      DATE NOT NULL,O_ORDERPRIORITY  CHAR(15) NOT NULL,   O_CLERK          CHAR(15) NOT NULL, O_SHIPPRIORITY   INTEGER NOT NULL,O_COMMENT        VARCHAR(79) NOT NULL);
CREATE TABLE LINEITEM ( L_ORDERKEY    INTEGER NOT NULL,L_PARTKEY     INTEGER NOT NULL, L_SUPPKEY     INTEGER NOT NULL,L_LINENUMBER  INTEGER NOT NULL, L_QUANTITY    DECIMAL(15,2) NOT NULL, L_EXTENDEDPRICE  DECIMAL(15,2) NOT NULL,L_DISCOUNT    DECIMAL(15,2) NOT NULL, L_TAX         DECIMAL(15,2) NOT NULL, L_RETURNFLAG  CHAR(1) NOT NULL,L_LINESTATUS  CHAR(1) NOT NULL, L_SHIPDATE    DATE NOT NULL,L_COMMITDATE  DATE NOT NULL, L_RECEIPTDATE DATE NOT NULL, L_SHIPINSTRUCT CHAR(25) NOT NULL,L_SHIPMODE     CHAR(10) NOT NULL,L_COMMENT      VARCHAR(44) NOT NULL);


select table_name,num_rows from user_tables order by num_rows desc;

mysql>select * from nation into outfile  '/home/yxx/tpch/nation.csv'  fields terminated by ',' optionally enclosed by '"' escaped by '"' lines terminated by '\r\n';

此条命令是将nation中的数据导出到nation.csv中，其中

fields terminated by ','　　　 ------字段间以,号分隔

optionally enclosed by '"'　　------字段用"号括起

escaped by '"'   　　　　　　------字段中使用的转义符为"

lines terminated by '\r\n';　　------行以\r\n结束

import data：
PG：
Copy region FROM '/usr/TPC-H/dbgen/region.csv' WITH DELIMITER AS '|';
Copy nation FROM '/usr/TPC-H/dbgen/nation.csv' WITH DELIMITER AS '|';
Copy part FROM '/usr/TPC-H/dbgen/part.csv' WITH DELIMITER AS '|';
Copy supplier FROM '/usr/TPC-H/dbgen/supplier.csv' WITH DELIMITER AS '|';
Copy customer FROM '/usr/TPC-H/dbgen/customer.csv' WITH DELIMITER AS '|';
Copy lineitem FROM '/usr/TPC-H/dbgen/lineitem.csv' WITH DELIMITER AS '|';
Copy partsupp FROM '/usr/TPC-H/dbgen/partsupp.csv' WITH DELIMITER AS '|';
Copy orders FROM '/usr/TPC-H/dbgen/orders.csv' WITH DELIMITER AS '|';

Copy region FROM '/usr/TPC-H/dbgen/region.tbl' WITH DELIMITER AS '|' NULL '';
Copy nation FROM '/usr/TPC-H/dbgen/nation.tbl' WITH DELIMITER AS '|' NULL '';
Copy part FROM '/usr/TPC-H/dbgen/part.tbl' WITH DELIMITER AS '|' NULL '';
Copy supplier FROM '/usr/TPC-H/dbgen/supplier.tbl' WITH DELIMITER AS '|' NULL '';
Copy customer FROM '/usr/TPC-H/dbgen/customer.tbl' WITH DELIMITER AS '|' NULL '';
Copy lineitem FROM '/usr/TPC-H/dbgen/lineitem.tbl' WITH DELIMITER AS '|' NULL '';
Copy partsupp FROM '/usr/TPC-H/dbgen/partsupp.tbl' WITH DELIMITER AS '|' NULL '';
Copy orders FROM '/usr/TPC-H/dbgen/orders.tbl' WITH DELIMITER AS '|' NULL '';

SQL Server：
BULK INSERT region FROM '/usr/TPC-H/dbgen/region.csv' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )
BULK INSERT nation FROM '/usr/TPC-H/dbgen/nation.csv' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )
BULK INSERT part FROM '/usr/TPC-H/dbgen/part.csv' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )
BULK INSERT supplier FROM '/usr/TPC-H/dbgen/supplier.csv' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )
BULK INSERT customer FROM '/usr/TPC-H/dbgen/customer.csv' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )
BULK INSERT lineitem FROM '/usr/TPC-H/dbgen/lineitem.csv' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )
BULK INSERT partsupp FROM '/usr/TPC-H/dbgen/partsupp.csv' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )
BULK INSERT orders FROM '/usr/TPC-H/dbgen/orders.csv' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )

BULK INSERT region FROM '/usr/TPC-H/dbgen/region.tbl' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )
BULK INSERT nation FROM '/usr/TPC-H/dbgen/nation.tbl' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )
BULK INSERT part FROM '/usr/TPC-H/dbgen/part.tbl' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )
BULK INSERT supplier FROM '/usr/TPC-H/dbgen/supplier.tbl' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )
BULK INSERT customer FROM '/usr/TPC-H/dbgen/customer.tbl' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )
BULK INSERT lineitem FROM '/usr/TPC-H/dbgen/lineitem.tbl' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )
BULK INSERT partsupp FROM '/usr/TPC-H/dbgen/partsupp.tbl' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )
BULK INSERT orders FROM '/usr/TPC-H/dbgen/orders.tbl' WITH( FIELDTERMINATOR = '|', ROWTERMINATOR = '\n' )


Oracle：
load data local infile '/usr/TPC-H/dbgen/region.tbl' into table region fields terminated by '|' lines terminated by '\n';
load data local infile '/home/Data/tpch-kit-master/dbgen/tbl/nation.tbl' into table nation fields terminated by '|' lines terminated by '\n';
load data local infile '/home/Data/tpch-kit-master/dbgen/tbl/customer.tbl' into table customer fields terminated by '|' lines terminated by '\n';
load data local infile '/home/Data/tpch-kit-master/dbgen/tbl/supplier.tbl' into table supplier fields terminated by '|' lines terminated by '\n';
load data local infile '/home/Data/tpch-kit-master/dbgen/tbl/part.tbl' into table part fields terminated by '|' lines terminated by '\n';
load data local infile '/home/Data/tpch-kit-master/dbgen/tbl/orders.tbl' into table orders fields terminated by '|' lines terminated by '\n';
load data local infile '/home/Data/tpch-kit-master/dbgen/tbl/partsupp.tbl' into table partsupp fields terminated by '|' lines terminated by '\n';
load data local infile '/home/Data/tpch-kit-master/dbgen/tbl/lineitem.tbl' into table lineitem fields terminated by '|' lines terminated by '\n';


dbca -silent -createDatabase -templateName General_Purpose.dbc -responseFile NO_VALUE -gdbname IMDB  -sid IMDB -createAsContainerDatabase TRUE -numberOfPDBs 1 -pdbName pdbIMDB -pdbAdminPassword SDUdell-123 -sysPassword SDUdell-123 -systemPassword SDUdell-123 -datafileDestination '/home/oracle/app/oracle/oradata' -recoveryAreaDestination '/home/oracle/flash_recovery_area' -redoLogFileSize 50 -storageType FS -characterset ZHS16GBK -nationalCharacterSet AL16UTF16 -totalMemory 51200 -databaseType OLTP  -emConfiguration NONE

Oracle with ctl files:
LOAD DATA INFILE '/usr/TPC-H/dbgen/region.tbl' 
INTO TABLE REGION
(
r_regionkey terminated by '|',
r_name terminated by '|',
r_comment terminated by '|'
)

LOAD DATA INFILE '/usr/TPC-H/dbgen/part.tbl'
INTO TABLE part 
(
    P_PARTKEY       terminated by '|',
    P_NAME          terminated by '|',
    P_MFGR          terminated by '|',
    P_BRAND         terminated by '|',
    P_TYPE          terminated by '|',
    P_SIZE          terminated by '|',
    P_CONTAINER     terminated by '|',
    P_RETAILPRICE   terminated by '|',
    P_COMMENT       terminated by '|'
)

LOAD DATA INFILE '/usr/TPC-H/dbgen/nation.tbl'   
INTO TABLE nation 
(
    N_NATIONKEY     terminated by '|',
    N_NAME          terminated by '|',
    N_REGIONKEY     terminated by '|',
    N_COMMENT       terminated by '|'
)

LOAD DATA INFILE '/usr/TPC-H/dbgen/supplier.tbl'   
INTO TABLE supplier 
(
    S_SUPPKEY       terminated by '|',
    S_NAME          terminated by '|',
    S_ADDRESS       terminated by '|',
    S_NATIONKEY     terminated by '|',
    S_PHONE         terminated by '|',
    S_ACCTBAL       terminated by '|',
    S_COMMENT       terminated by '|'
)

LOAD DATA INFILE '/usr/TPC-H/dbgen/partsupp.tbl'   
INTO TABLE partsupp  
(
    PS_PARTKEY      terminated by '|',
    PS_SUPPKEY      terminated by '|',
    PS_AVAILQTY     terminated by '|',
    PS_SUPPLYCOST   terminated by '|',
    PS_COMMENT      terminated by '|'
)

LOAD DATA INFILE '/usr/TPC-H/dbgen/customer.tbl'   
INTO TABLE customer 
(
    C_CUSTKEY       terminated by '|',
    C_NAME          terminated by '|',
    C_ADDRESS       terminated by '|',
    C_NATIONKEY     terminated by '|',
    C_PHONE         terminated by '|',
    C_ACCTBAL       terminated by '|',
    C_MKTSEGMENT    terminated by '|',
    C_COMMENT       terminated by '|'
)

LOAD DATA INFILE '/usr/TPC-H/dbgen/orders.tbl'   
INTO TABLE orders fields terminated by "|" TRAILING NULLCOLS
(
    O_ORDERKEY      terminated by '|',
    O_CUSTKEY       terminated by '|',
    O_ORDERSTATUS   terminated by '|',
    O_TOTALPRICE    terminated by '|',
    O_ORDERDATE     DATE "YYYY-MM-DD",
    O_ORDERPRIORITY terminated by '|',
    O_CLERK         terminated by '|',
    O_SHIPPRIORITY  terminated by '|',
    O_COMMENT       terminated by '|'
)

LOAD DATA INFILE '/usr/TPC-H/dbgen/lineitem.tbl'   
INTO TABLE lineitem  fields terminated by "|" TRAILING NULLCOLS
(
        L_ORDERKEY              terminated by "|" ,
        L_PARTKEY               terminated by "|",
        L_SUPPKEY               terminated by "|",
        L_LINENUMBER    terminated by "|",
        L_QUANTITY              terminated by "|",
        L_EXTENDEDPRICE terminated by "|",
        L_DISCOUNT              terminated by "|",
        L_TAX                   terminated by "|",
        L_RETURNFLAG    CHAR(1) ,
        L_LINESTATUS    CHAR(1) ,
        L_SHIPDATE              DATE "YYYY-MM-DD" ,
        L_COMMITDATE    DATE "YYYY-MM-DD" ,
        L_RECEIPTDATE   DATE "YYYY-MM-DD" ,
        L_SHIPINSTRUCT  CHAR(25) ,
        L_SHIPMODE              CHAR(10) ,
        L_COMMENT               CHAR(44)
)

use those ctl files:
sqlldr userid=SYSTEM/password control = "test.ctl"


Alter KEY:
-- For table REGION
ALTER TABLE  REGION ADD PRIMARY KEY (R_REGIONKEY);

-- For table NATION
ALTER TABLE NATION ADD PRIMARY KEY (N_NATIONKEY);
ALTER TABLE NATION ADD FOREIGN KEY  (N_REGIONKEY) REFERENCES REGION;

-- For table PART
ALTER TABLE PART ADD PRIMARY KEY (P_PARTKEY);

-- For table SUPPLIER
ALTER TABLE SUPPLIER ADD PRIMARY KEY (S_SUPPKEY);
ALTER TABLE SUPPLIER ADD FOREIGN KEY (S_NATIONKEY) REFERENCES NATION;

-- For table PARTSUPP
ALTER TABLE PARTSUPP ADD PRIMARY KEY (PS_PARTKEY, PS_SUPPKEY);

-- For table CUSTOMER
ALTER TABLE CUSTOMER ADD PRIMARY KEY (C_CUSTKEY);
ALTER TABLE CUSTOMER ADD FOREIGN KEY (C_NATIONKEY) REFERENCES NATION;

-- For table LINEITEM
ALTER TABLE LINEITEM ADD PRIMARY KEY (L_ORDERKEY, L_LINENUMBER);

-- For table ORDERS
ALTER TABLE ORDERS ADD PRIMARY KEY (O_ORDERKEY);

-- For table PARTSUPP
ALTER TABLE PARTSUPP ADD FOREIGN KEY  (PS_SUPPKEY) REFERENCES SUPPLIER;
ALTER TABLE PARTSUPP ADD FOREIGN KEY  (PS_PARTKEY) REFERENCES PART;

-- For table ORDERS
ALTER TABLE ORDERS ADD FOREIGN KEY  (O_CUSTKEY) REFERENCES CUSTOMER;

-- For table LINEITEM
ALTER TABLE LINEITEM ADD FOREIGN KEY  (L_ORDERKEY) REFERENCES ORDERS;
ALTER TABLE LINEITEM ADD FOREIGN KEY  (L_PARTKEY, L_SUPPKEY) REFERENCES PARTSUPP;

COMMIT WORK;


SELECT     l_returnflag,    l_linestatus,    SUM (l_quantity) AS sum_qty,    SUM (l_extendedprice) AS sum_base_price,    SUM (l_extendedprice * (1 - l_discount)) AS sum_disc_price,    SUM (l_extendedprice * (1 - l_discount) * (1 + l_tax)) AS sum_charge,    AVG (l_quantity) AS avg_qty,    AVG (l_extendedprice) AS avg_price,    AVG (l_discount) AS avg_disc,    COUNT (*) AS count_order  FROM     lineitem WHERE    l_shipdate <= DATE '1998-12-01' - INTERVAL '90' GROUP BY     l_returnflag,    l_linestatus  ORDER BY    l_returnflag,    l_linestatus  LIMIT 10;

SQL Server:
sqlcmd -S localhost -U SA -P password                              //登入数据库

sqlcmd -S localhost -U SA -P password -d master -i 2.sql   //执行查询文件

SET SHOWPLAN_TEXT ON
GO
set statistics profile on 
GO

Oracle:
dbca -silent -createDatabase -templateName General_Purpose.dbc -responseFile NO_VALUE -gdbname imdb2  -sid imdb2 -createAsContainerDatabase TRUE -numberOfPDBs 1 -pdbName pdbimdb2 -pdbAdminPassword password -sysPassword password -systemPassword password -datafileDestination '/home/oracle/app/oracle/oradata' -recoveryAreaDestination '/home/oracle/flash_recovery_area' -redoLogFileSize 50 -storageType FS -characterset ZHS16GBK -nationalCharacterSet AL16UTF16 -totalMemory 10240 -databaseType OLTP  -emConfiguration NONE


sqlldr userid=SYSTEM/password control = "test.ctl"

change str:
sed -i "s#\\\\\"#\\'#g" company_name.csv


oracle使用：

登录：
切换oracle用户：su - oracle

查看库：echo $ORACLE_SID
若是IMDB，那就是IMDB的数据
若是tpch，那就是tpch数据

修改库：
进入：/home/oracle
修改配置文件：vim .bash_profile
找到ORACLE_SID，修改成tpch或者IMDB
然后使用source .bash_profile使配置文件生效


输入：sqlplus -> system ->密码：password
可以使用了

oracle 安装IMDB数据集:

CREATE TABLE company_type( id Integer NOT NULL, kind Varchar(32) NOT NULL);
ALTER TABLE company_type ADD CONSTRAINT pk_c_t PRIMARY KEY (id);

CREATE TABLE company_name( id Integer NOT NULL, name Varchar(255) NOT NULL, country_code Varchar(255), imdb_id Integer, name_pcode_nf Varchar(5), name_pcode_sf Varchar(5), md5sum Varchar(32));
ALTER TABLE company_name ADD CONSTRAINT pk_c_n PRIMARY KEY (id);

CREATE TABLE company_name(  id Integer NOT NULL, name Varchar2(1000) NOT NULL, country_code Varchar(255), imdb_id Integer, name_pcode_nf Varchar(5), name_pcode_sf Varchar(5), md5sum Varchar(32));


LOAD DATA INFILE '/usr/imdb/company_type.csv'   
INTO TABLE company_type fields terminated by ',' TRAILING NULLCOLS
(
    id       terminated by ',',
    kind          terminated by ','
)
