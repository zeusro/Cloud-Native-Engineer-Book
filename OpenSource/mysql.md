## "error while loading shared libraries: xxx.so.x" 错误的原因和解决办法

原因一般有两个, 一个是操作系统里确实没有包含该共享库(lib*.so.*文件)或者共享库版本不对, 遇到这种情况那就去网上下载并安装上即可.

另外一个原因就是已经安装了该共享库, 但执行需要调用该共享库的程序的时候, 程序按照默认共享库路径找不到该共享库文件.

```bash
ls /usr/local/mysql/lib | grep  libmysqlclient

1.先确认/usr/local/mysql/lib/下是否有libmysqlclient.so.18这个文件.
ll /usr/local/mysql/lib/|grep libmysqlclient.so.18
2.修改ld.so.conf文件
echo "/usr/local/mysql/lib " > >/etc/ld.so.conf
```


然后执行ldconfig使其生效

ldconfig

"error while loading shared libraries: xxx.so.x" 错误的原因和解决办法
https://blog.csdn.net/sahusoft/article/details/7388617


## [ERROR] You have enabled the binary log, but you haven't provided the mandatory server-id.

```bash
[root@mysql bin]# vi /etc/my.cnf
#add
[mysqld]
log-bin=mysql-bin
server-id=1
```

In MySQL 5.7.2 and earlier, if you start a master server without using --server-id to set its ID, the default ID is 0. In this case, the master refuses connections from all slaves, slaves refuse to connect to the master, and the server sets the server_id system variable to 1. In MySQL 5.7.3 and later, the --server-id must be used if binary logging is enabled, and a value of 0 is not changed by the server. If you specify --server-id without an argument, the effect is the same as using 0. In either case, if the server_id is 0, binary logging takes place, but slaves cannot connect to the master, nor can any other servers connect to it as slaves. (Bug #11763963, Bug #56718)

MySQL 5.7.3 版本后，如果要开启二进制日志，必须同时开启 --server-id 选项。

server-id 不要设置为0（不支持主从复制），应该设置为 1 或者其他值。


https://blog.csdn.net/DBDoctor/article/details/52422637

MySQL Server-id踩到的坑

https://www.cnblogs.com/gomysql/p/5852607.html
