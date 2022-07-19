## 2022.6.19 星期日

上次出现systemtap报错后，查阅了很多资料。

有如下两种猜想：

1.systemtap安装过程中缺少有关包；

2.systemtap更新版本后，不再支持process().mark()来标记程序提供的标记点。

有关于第一种的猜想，我尝试了很多次安装其他的包，有的安装了，有的提示已经安装了。运行stap脚本程序后，依然没有成功。

第二种猜想，是因为查看最近的一些资料，发现对于systemtap中语句的介绍，比较少有process()这个功能了，所以怀疑可能是版本不支持了，但是也没有找到很好的方法解决。



systemtap安装相关包的代码：

```
yum install -y kernel-devel-$(uname -r) \

kernel-debuginfo-$(uname -r) \

kernel-debuginfo-common-$(uname -m)-$(uname -r)

```
