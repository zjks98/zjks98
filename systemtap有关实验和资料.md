## 7月4号周一

查找systemtap的资料

stap -v -e 'probe process("/usr/local/pgsql/bin/postgres").mark("query-start") {println(gettimeofday_s())}'

PG官方文档中，有关探测的资料：

https://www.postgresql.org/docs/14/dynamic-trace.html
