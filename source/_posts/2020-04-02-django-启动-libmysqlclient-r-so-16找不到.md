---
title: django 启动 libmysqlclient_r.so.16找不到
date: 2020-04-02 13:51:29
urlname: django-libmysqlclient-error
categories:
- django
tags:
- 开发
- django
- mysql
---
>问题：开发环境由于yum安装的mysql版本太低，卸载后从mysql官网下载rpm进行安装，正常设置mysql之后，在启动Django时，提示libmysqlclient_r.so.16找不到。

<!--more -->

#### 问题描述
>django.core.exceptions.ImproperlyConfigured: Error loading MySQLdb module: libmysqlclient_r.so.16: cannot open shared object file: No such file or directory.

在网上找了不少回答，发现都是扯的很，最后发现的问题是，在通过rpm方式安装mysql时，使用的是mysql-rpm-bundle的版本，当时只安装了mysql-client和mysql-server。

完整的安装包：
```bash
- -rw-r--r--  1 root root  285470720 Mar 31 19:53 MySQL-5.6.46-1.el6.x86_64.rpm-bundle.tar
- -rw-r--r--  1 7155 31415  35528000 Sep 29  2019 MySQL-client-5.6.46-1.el6.x86_64.rpm
- -rw-r--r--  1 7155 31415   6191888 Sep 29  2019 MySQL-devel-5.6.46-1.el6.x86_64.rpm
- -rw-r--r--  1 7155 31415  98136984 Sep 29  2019 MySQL-embedded-5.6.46-1.el6.x86_64.rpm
- -rw-r--r--  1 7155 31415  80628348 Sep 29  2019 MySQL-server-5.6.46-1.el6.x86_64.rpm
- -rw-r--r--  1 7155 31415   3722376 Sep 29  2019 MySQL-shared-5.6.46-1.el6.x86_64.rpm
- -rw-r--r--  1 7155 31415   3969752 Sep 29  2019 MySQL-shared-compat-5.6.46-1.el6.x86_64.rpm
- -rw-r--r--  1 7155 31415  57279436 Sep 29  2019 MySQL-test-5.6.46-1.el6.x86_64.rpm
```

缺少libmysqlclient_r.so.16是因为MySQL-shared-compat-5.6.46-1.el6.x86_64.rpm没有安装，直接安装即可
```bash
rpm -ivh MySQL-shared-compat-5.6.46-1.el6.x86_64.rpm
warning: MySQL-shared-compat-5.6.46-1.el6.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
Preparing...                ########################################### [100%]
   1:MySQL-shared-compat    ########################################### [100%]

[root@dpctest upload]# find / -name libmysqlclient_r.so.16
/usr/lib64/libmysqlclient_r.so.16
```