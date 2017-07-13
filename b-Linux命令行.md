#### 常用快捷键
```bash
[**@** /]$ ctrl+l #清屏
[**@** /]$ ctrl+u  #清除命令前部分
[**@** /]$ ctrl+k  #清除命令后部分
[**@** /]$ ctrl+d #退出当前编辑
[**@** /]$ ctrl+c  #取消执行命令
[**@** /]$ ctrl+z  #暂停命令
[**@** /]$ fg  #恢复中止命令
[**@** /]$ jobs #查看暂停的命令
[**@** /]$ ctrl+s  #锁住屏幕
[**@** /]$ ctil+q  #恢复解锁
[**@** /]$ exit  #注销linux
[**@** /]$ shutdown  #关机
[**@** /]$ man page  #在线求助
```
#### 基本的bash shell命令
>  文件目录创建、查看命令。'.'当前目录，'..'上一层目录，'-'前一个工作目录，'~'用户当前目录,'\'进入根目录。这里文件适应通配符，'?'匹配单个任意字符，'\*'匹配个或多个字符，[1-8]匹配1到8，[a,b]匹配a或b。

```bash
[**@** /]$ ls -l 1.txt  #查询文件列表，文件名称可作为过滤器
[**@** /]$ touch 1.txt #创建文件
[**@** /]$ cp 1.txt 2.txt #复制文件
[**@** /]$ mv 1.txt 2.txt #移动文件
[**@** /]$ rm 2.txt #删除文件
[**@** /]$ mkdir dir1 #创建目录
[**@** /]$ rmdir -rf dir1 #删除目录，强制删除目录下面文件
[**@** /]$ stat 1.txt #查看文件详细信息
[**@** /]$ file 1.txt #查看文件类型
[**@** /]$ cat 1.txt #查看文件信息
[**@** /]$ tail -f 1.txt #实时监控文件
[**@** /]$ head 1.txt #查看文件信息
[**@** /]$ pwd  #查看当前目录
```
#### 更多的bash shell命令
> 进程、磁盘信息查看

```bash
[**@** /]$ ps -ef | grep java/zookeeper/tomcat #查看java或tomcat或zookeeper进程
[**@** /]$ kill 1122 #通过PID删除进程
[**@** /]$ killall http* #通过进程名删除进程，适用过滤器
[**@** /]$ mount #输出挂载媒体
[**@** /]$ umount /home/rich/mnt #卸载设备或目录
[**@** /]$ df -h #查看磁盘空间
[**@** /]$ sort -n 1.txt #通过数字排序文件内容
[**@** /]$ sort -t ':' -k 3 -nr /etc/passwd #通过冒号分段，用第每行第3段排序,r命令降序
[**@** /]$ grep -n [ef] 1.txt #查找文档中有e或f，正则表达式
[**@** /]$ tar -cvf test.tar test/ #创建一个包含test目录的压缩包
[**@** /]$ tar -tf test.tar #列出tar文件内容
[**@** /]$ tar -xvf test.tar #提取归档文件
```
#### 使用Linux环境变量
> 局部变量在./bash_profile和./bashrc目录下面，系统变量在etc/profile和etc/bashrc下面。其中profile是非交互式，例如su命令，bash是交互式。

```bash
[**@** /]$ printenv或env #查询所有全局变量，大写
[**@** /]$ set #查看所有变量，包含局部环境变量
[**@** /]$ test=testing #设置局部环境变量
[**@** /]$ echo $test #输出局部环境变量
[**@** /]$ bash #创建一个子shell
[**@** /]$ export test #导出到全局变量中
[**@** /]$ unset test #删除环境变量
[**@** /]$ mytest(one two three four five) #设置数组
[**@** /]$ echo $(mytest[2]) #输出three
[**@** /]$ alias li='ls -l' #命令别名
[**@** /]$ which ls  #查看别名
[**@** /]$ alias  #查看所有别名
[**@** /]$ PATH=$PATH:/tmp/ #添加tmp路径到执行环境
[**@** /]$ source /etc/profile或 . /etc/profile  #修改的系统变量即时生效
```
#### 理解Linux文件权限
> 添加目录，会复制/etc/skel目录到/home目录下，生成用户目录。

```bash
[**@** /]$ sudo useradd -m test #新建用户账户，通过sudo用root权限运行,-m创建home文件
[**@** /]$ visudo  #如果用户没有sudo权限，编辑文档在ALLOW root to run any下面添加tomcat ALL=(ALL) ALL然后保存
[**@** /]$ userdel -r test #删除用户，同事删除HOME和mail目录
[**@** /]$ passwd test #修改密码
[**@** /]$ groupadd shared #创建用户组
[**@** /]$ usermod -G shared test #把用户添加到分组
[**@** /]$ groupmod -n sharing shared #修改组名
[**@** /]$ groups  #查看当前用户组
[**@** /]$ groups tomcat  #查看tomcat用户组
```
> 文件权限设置，文件读、写（编辑和删除）、执行权限，目录读（列出目录文件）、写（目录下创建和删除文件）、执行（搜索和进入目录）。

```bash
[**@** /]$ umask #默认文件权限
[**@** /]$ chmod 760 newfile #档案权限，修改文件权限，八进制
[**@** /]$ chown -R tomcat.server newfile #修改文件属主和属组，递归修改目录以及文件
```
#### 安装软件程序
> aptitude库位置存储在文件/etc/apt/source.list中。

```bash
[**@** /]$ aptitude show grub2-theme-mint #查看安装包信息
[**@** /]$ dpkg #search /boot/grub/linuxmit.png #反向查看文件属于哪个安装包
[**@** /]$ aptitude search wine #安装软件包，显示i wine -**已安装，显示p wine -**未安装
[**@** /]$ aptitude install wine #开始安装，再用search查看是否安装成功
[**@** /]$ aptitude safe-upgrade #升级所有安装包
[**@** /]$ aptitude purge wine #卸载软件，删除相关数据与配置，可用remove仅卸载
```
#### 使用编辑器
> `一般模式`可以（上下左右、删除、复制、粘贴），`编辑模式`可以（编辑文件内容），`命令模式`可以（搜寻、读取、存盘）。

```bash
#般模式下的命令
[**@** /]$ dd#删除整行G#移动到末行&gg#移动到第一行&yy#复制一行&p#粘贴&u#复原前一个动作&ctrl+r#重复前一个动作
[**@** /]$ i #进入编辑模式
[**@** /]$ esc #退出到一般模式
[**@** /]$ : #进入命令模式
#命令模式下的命令
[**@** /]$ q!#不保存退出&wq#保存退出&w#存储数据&/word#向下寻找word&?word#向上寻找word&n#重复前一个寻找动作&
N#反向重复前一个寻找动作&set nu#设置行号&set nonu#取消行号
```

