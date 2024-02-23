+++
title = 'RPM方式安装MySQL'
date = 2024-02-23T16:14:54+08:00
draft = false
+++

# RPM方式安装MySQL

最近浪子尝试使用mycat做MySQL的读写分离和分表分库, 因此搭建了几台虚拟机来做操作.话不多说,我们现在centos7上安装MySQL.

据说centos7上面直接用`yum`的方式安装MySQL会失败,那么我就直接使用手动的方式安装.

### 下载需要的安装包

我选择的是国内的华为镜像, 速度比较快一些. 分别下载 `common` `libs` `client` `server` 4个RPM包.安装的MySQL版本可以在[华为镜像](https://mirrors.huaweicloud.com/mysql/Downloads/MySQL-5.7/ )里面自己选择,我选择的MySQL5.7.

```
wget https://mirrors.huaweicloud.com/mysql/Downloads/MySQL-5.7/mysql-community-common-5.7.30-1.el6.i686.rpm

wget https://mirrors.huaweicloud.com/mysql/Downloads/MySQL-5.7/mysql-community-libs-5.7.30-1.el6.x86_64.rpm

wget https://mirrors.huaweicloud.com/mysql/Downloads/MySQL-5.7/mysql-community-client-5.7.30-1.el6.x86_64.rpm

wget https://mirrors.huaweicloud.com/mysql/Downloads/MySQL-5.7/mysql-community-server-5.7.30-1.el6.x86_64.rpm
```



### 按照顺序安装4个RPM包

记住,一点要按照顺序安装,因为包于包之间有依赖关系.

```
rpm -ivh mysql-community-libs-5.7.30-1.el6.x86_64.rpm --force --nodeps

rpm -ivh mysql-community-client-5.7.30-1.el6.x86_64.rpm  --force --nodeps

rpm -ivh mysql-community-client-5.7.30-1.el6.x86_64.rpm  --force --nodeps

rpm -ivh mysql-community-server-5.7.30-1.el6.x86_64.rpm --force --nodeps
```

我在安装的时候, 最开始没有加上参数`--force --nodeps` ,然后会安装失败,会有类似于这样的错误:

```
[root@localhost mysql]# rpm -ivh mysql-community-libs-5.7.30-1.el6.x86_64.rpm
警告：mysql-community-libs-5.7.30-1.el6.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
错误：依赖检测失败：
	mysql-community-common(x86-64) >= 5.7.9 被 mysql-community-libs-5.7.30-1.el6.x86_64 需要
```

![image-20201224130527553](/Users/shadow/Pictures/typora/image-20201224130527553.png)

所以推荐安装时加上依赖.



### 启动MySQL

```
service mysqld start
```



### 修改初始密码

默认安装好MySQL后,密码会在日志里面记录.可以通过这条命令查看初始密码:

```
cat /var/log/mysqld.log |grep password
```

你看到的内容类似于下面这样:

![image-20201224130826392](/Users/shadow/Pictures/typora/image-20201224130826392.png)



#### 登录MySQL

通过下面的命令登录MySQL:

```
mysql -uroot -p 
```

输入上面找到的密码.



#### 修改root登录密码

可以在登录MySQL后,用一下命令修改登录密码:

```
set password = password('123456');
# 刷新权限
flush privileges;
```

> 有的时候会这个命令会执行失败,是因为你设置的密码过于简单.可以通过2种方式解决:(正式环境推荐使用复杂的密码)
>
> 1. 设置一个很复杂的密码
>
> 2. 降低密码的校验强度,使用一些命令:
>
>    ```
>    # 设置密码校验强度为最低级别,
>    set global validate_password_policy=0;
>    # 设置密码长度最少为4个字符
>    set global validate_password_length=4;
>    # 刷新权限
>    flush privileges;
>    # 然后在执行上面的设置密码命令
>    ```



到此为止,我们的MySQL就安装成功啦~ enjoy it~
