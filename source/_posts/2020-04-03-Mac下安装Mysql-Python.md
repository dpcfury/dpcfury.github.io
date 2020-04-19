---
title: Mac下安装Mysql-Python
date: 2020-04-03 11:00:34
urlname: mac-mysql-python-installation
categories:
- django
tags:
- 开发
- django
- python
---
>历史原因，接受项目的运行和调试基本放在开发测试机上，买了Pycharm之后，希望能借助本地的调试功能避免在开机上测试的时间成本，但是在本地安装依赖的过程中，发现着实遇到不少的坑，这次先说说安装的Mysql-python这个库。

<!-- more-->
#### Mysql-python
MySQL-python又叫MySQLdb，是Python连接MySQL最流行的一个驱动，很多框架都也是基于此库进行开发，遗憾的是它只支持 Python2.x，而且安装的时候有很多前置条件，因为它是基于C开发的库，这意味着，你在不同的开发测试环境，可能需要安装额外的上游依赖。

系统环境：
```bash
$ python -V
$ Python 2.7.10
```

```bash
$ sw_vers
ProductName:  Mac OS X
ProductVersion: 10.14.6
BuildVersion:   18G103
```

#### 问题
首先使用Pycharm安装失败，然后转为手动到命令行安装，可以报错信息：
```bash
dupengcheng@dpc-MacBook ~ pip install MySQL-python
DEPRECATION: Python 2.7 will reach the end of its life on January 1st, 2020. Please upgrade your Python as Python 2.7 won't be maintained after that date. A future version of pip will drop support for Python 2.7.
Collecting MySQL-python
  Downloading https://files.pythonhosted.org/packages/a5/e9/51b544da85a36a68debe7a7091f068d802fc515a3a202652828c73453cad/MySQL-python-1.2.5.zip (108kB)
    100% |████████████████████████████████| 112kB 3.1kB/s
    Complete output from command python setup.py egg_info:
    sh: mysql_config: command not found
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/private/var/folders/8h/kmnz1jj95s9g674q6vd1bmv40000gp/T/pip-install-DNT2Pa/MySQL-python/setup.py", line 17, in <module>
        metadata, options = get_config()
      File "setup_posix.py", line 43, in get_config
        libs = mysql_config("libs_r")
      File "setup_posix.py", line 25, in mysql_config
        raise EnvironmentError("%s not found" % (mysql_config.path,))
    EnvironmentError: mysql_config not found

    ----------------------------------------
```

和Java中不同，python的mysql连接驱动依赖了部分c库实现，推测这里的mysql_config也是某个c库提供，于是goole一波，mysql_config 在 Mac 下是由 mysql-connector-c 提供的。
> diss 一波，联动的依赖管理和坑，给开发和后续维护埋了多少坑，2.7语言即将不支持，Mysql-Python也过于老旧，这种后续在客户侧升级演进隐藏的成本极高。

```bash
$ brew install mysql # 非本地数据库可以不安装
$ brew install mysql-connector-c
```

安装mysql-connector-c之后，重新pip install
```bash
DEPRECATION: Python 2.7 will reach the end of its life on January 1st, 2020. Please upgrade your Python as Python 2.7 won't be maintained after that date. A future version of pip will drop support for Python 2.7.
Collecting MySQL-python
  Using cached https://files.pythonhosted.org/packages/a5/e9/51b544da85a36a68debe7a7091f068d802fc515a3a202652828c73453cad/MySQL-python-1.2.5.zip
    Complete output from command python setup.py egg_info:
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/private/var/folders/8h/kmnz1jj95s9g674q6vd1bmv40000gp/T/pip-install-bQBOWl/MySQL-python/setup.py", line 17, in <module>
        metadata, options = get_config()
      File "setup_posix.py", line 53, in get_config
        libraries = [ dequote(i[2:]) for i in libs if i.startswith(compiler_flag("l")) ]
      File "setup_posix.py", line 8, in dequote
        if s[0] in "\"'" and s[0] == s[-1]:
    IndexError: string index out of range

    ----------------------------------------
```

修改/usr/local/Cellar/mysql-connector-c/6.1.11/bin/mysql_config
```bash
# Create options
113 libs="-L$pkglibdir"
114 # libs="$libs -l "
115 libs="$libs -lmysqlclient -lssl -lcrypto"
```
再次pip isstall 一次，又出了新问题，提示库缺少，命名已经安装
```bash
13 warnings generated.
    cc -bundle -undefined dynamic_lookup -Wl,-F. build/temp.macosx-10.14-intel-2.7/_mysql.o -L/usr/local/Cellar/mysql-connector-c/6.1.11/lib -lmysqlclient -lssl -lcrypto -o build/lib.macosx-10.14-intel-2.7/_mysql.so
    ld: library not found for -lssl
    clang: error: linker command failed with exit code 1 (use -v to see invocation)
    error: command 'cc' failed with exit status 1
```
没法了，继续google解决方案，尝试添加连接
```bash
export LDFLAGS="-I/usr/local/opt/openssl/include -L/usr/local/opt/openssl/lib"
```
再次安装，发现权限问题,加上sudo:

```bash
sudo env LDFLAGS="-I/usr/local/opt/openssl/include -L/usr/local/opt/openssl/lib" pip install Mysql-python
```

```bash
Collecting Mysql-python
  Downloading https://files.pythonhosted.org/packages/a5/e9/51b544da85a36a68debe7a7091f068d802fc515a3a202652828c73453cad/MySQL-python-1.2.5.zip (108kB)
    100% |████████████████████████████████| 112kB 5.3kB/s
Installing collected packages: Mysql-python
  Running setup.py install for Mysql-python ... done
Successfully installed Mysql-python-1.2.5
```

#### 总结
python在平台无关性上确实不如Java，环境的依赖管理尽管有pip这样的工具，但是实际的对操作系统依赖还是无法避免，在做代码移植上确实会存在问题。此外，遇到这种老版本依赖的升级，相比也会遇到更多的坑，这个在技术规划阶段就需要考虑，用的时候一时爽，后面维护火葬场。

