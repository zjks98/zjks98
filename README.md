## 7月4号周一

查找systemtap的资料

stap -v -e 'probe process("/usr/local/pgsql/bin/postgres").mark("query-start") {println(gettimeofday_s())}'

