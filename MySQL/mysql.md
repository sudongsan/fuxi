## 索引：

聚族索引和非聚族索引

mysiam引擎使用的是非聚族索引，B+树的叶节点存的是数据记录的地址，根据地址去读取相应的数据。

Innodb引擎使用的是聚族索引，其数据结构也是B+树，管理存储空间最基本的单位是页，大小是16kb，非叶子节点保存的是页目录，叶子节点保存了完整的数据记录，页之间是双向链表连接，页里面的用户记录是单向链表连接。

聚族索引的Key是数据表的主键，其他索引叶子节点存储的不是地址，而是主键的值。使用主键索引的时候根据key的值可以在叶节点找到对应的数据，而使用其他辅助索引，先找到主键的值，再进行一次回表，去主键索引搜索。所以主键应该尽量是递增，不应该过长，避免B+树频繁分裂。



## 锁：

行锁：行锁，间隙锁，当前行+间隙

表锁

## 面经

https://mp.weixin.qq.com/s/uIFHe8Myb0RvNxtAivSzOA



## MySQL 是如何解决幻读的

https://www.cnblogs.com/wdy1184/p/10655180.html



## MVCC

https://www.jianshu.com/p/cef49aeff36b

https://www.jb51.net/article/159122.htm



## MySQL安装：centos7+mysql5.7

#### 镜像下载：

https://blog.csdn.net/qq_41113081/article/details/94455417

#### 安装步骤：

https://blog.csdn.net/qq_15092079/article/details/81629238

#### 卸载MySQL：

https://www.cnblogs.com/nicknailo/articles/8563456.html

**1、使用以下命令查看当前安装mysql情况，查找以前是否装有mysql**

```shell
`rpm -qa|``grep` `-i mysql`
```

可以看到如下图的所示：

显示之前安装了：

​     MySQL-client-5.5.25a-1.rhel5

​     MySQL-server-5.5.25a-1.rhel5

**2、停止mysql服务、删除之前安装的mysql**

删除命令：`rpm -e –nodeps 包名`

```shell
`rpm -ev MySQL-client-5.5.25a-1.rhel5 ``rpm -ev MySQL-server-5.5.25a-1.rhel5`
```

如果提示依赖包错误，则使用以下命令尝试

```shell
`rpm -ev MySQL-client-5.5.25a-1.rhel5 --nodeps`
```

如果提示错误：`error: %preun(xxxxxx) scriptlet failed, exit status 1`

则用以下命令尝试：

```shell
`rpm -e --noscripts MySQL-client-5.5.25a-1.rhel5`
```

**3、查找之前老版本mysql的目录、并且删除老版本mysql的文件和库**

```shell
`find` `/ -name mysql`
```

查找结果如下：

```shell
`find` `/ -name mysql ` `/var/lib/mysql``/var/lib/mysql/mysql``/usr/lib64/mysql`
```

删除对应的mysql目录

```shell
`rm` `-rf ``/var/lib/mysql``rm` `-rf ``/var/lib/mysql``rm` `-rf ``/usr/lib64/mysql`
```

具体的步骤如图：查找目录并删除

**注意：**卸载后/etc/my.cnf不会删除，需要进行手工删除

```shell
`rm` `-rf ``/etc/my``.cnf`
```

**4、再次查找机器是否安装mysql**

```shell
`rpm -qa|``grep` `-i mysql`
```

## 主从同步配置搭建

https://blog.csdn.net/qq_15092079/article/details/81672920

## MySQL读写分离

MySql的读写分离简介

https://www.jianshu.com/p/2d8ff87d030c

你们有没有做 MySQL 读写分离？如何实现 MySQL 的读写分离？MySQL 主从复制原理的是啥？如何解决 MySQL 主从同步的延时问题？

https://www.jianshu.com/p/8ae6e2d9a254

搭建：

https://blog.51cto.com/13784264/2174492

![1570098281344](C:\Users\WPC\AppData\Roaming\Typora\typora-user-images\1570098281344.png)

![1570098298891](C:\Users\WPC\AppData\Roaming\Typora\typora-user-images\1570098298891.png)

![1570098333698](C:\Users\WPC\AppData\Roaming\Typora\typora-user-images\1570098333698.png)

## 数据库优化

- 优化查询操作，比如建立索引，expalain分析查询计划
- 开启慢查询日志
- show processlist
- 加缓存
- 搭建集群，实现读写分离
- 分库分表