##### 安装Mysql

```bash
#下载安装包http://dev.mysql.com/downloads/mysql/5.6.html#downloads
#解压
[**@** /]$ tar -zxvf mysql-5.6.33-linux-glibc2.5-x86_64.tar.gz
#复制解压后的mysql目录
[**@** /]$ cp -r mysql-5.6.33-linux-glibc2.5-x86_64 /usr/local/mysql
#添加用户组
[**@** /]$ groupadd mysql
#添加用户到用户组mysql
[**@** /]$ useradd -g mysql mysql
#安装
[**@** /]$ cd /usr/local/mysql
>  mkdir ./data/mysql
>  chown -R mysql:myssql ./  #修改当前目录拥有者为mysql用户
>  ./scripts/mysql_install_db --user=mysql --datadir=/usr/local/mysql/data/mysql   #安装mysql
>  cp support-files/mysql.server /etc/init.d/mysqld  #添加开机启动
>  chmod 755 /etc/init.d/mysqld
>  cp support-files/my-default.conf /etc/my.cnf
#修改启动脚本
[**@mysql /]$ vi /etc/init.d/mysqld  #basedir=/usr/local/mysql/ datadir=/usr/local/mysql/data/mysql
#加入环境变量
[**@mysql /]$ export PATH=$PATH:/usr/local/mysql/bin
> source /etc/profile
[**@mysql /]$ service mysqld start   #启动服务
[**@mysql /]$ ./bin/mysqladmin -u root password 'password'  #修改密码
[**@mysql /]$ ./mysql/bin/mysql -uroot   #测试连接
[**@mysql /]$ service mysqld stop  #关闭mysql
[**@mysql /]$ service mysqld status  #查看运行状态
[**@mysql /]$ service mysqld restart  #重启mysql
```

###### 常见问题

> 1. 没有远程连接权限
>
>    `GRANT ALL PRIVILEGES ON *.* TO ‘root’@'%’ IDENTIFIED BY ‘youpassword’ WITH GRANT OPTION;`
>
> 2. 安装-bash: ./scripts/mysql_install_db: /usr/bin/perl: bad interpreter: 没有那个文件或目录
>
>    ` yum -y install perl perl-devel`
>
> 3. Installing MySQL system tables..../bin/mysqld: error while loading shared libraries: libaio.so.1: cannot open shared object file: No such file or directory
>
>    `yum -y install libaio-devel`
>
> 4. FATAL ERROR: please install the following Perl modules before executing /usr/local/mysql/scripts/mysql_install_db:初始化mysql数据库提示缺少Data:dumper模块解决方法，解决方案
>
>    `yum-y install autoconf `

##### Mysql客户端界面，

```mysql
[**@** /]$ mysql -u root -p  #连接到服务器
mysql>  GRANT SELECT,INSERT,DELETE,UPDATE ON test.* TO test IDENTIFIED by 'password'
```

