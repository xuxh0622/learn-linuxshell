##### 构建基本脚本

> 引号作用：test=\`date\` 这里反引号把整个命令行命令圈起来，test="$PATH"双引号看到美元符会解释成变量，test='date'单引号保持原状。

```bash
[**@** /]$ touch test
[**@** /]$ vim test
#!/bin/bash  #指定使用的shell
#this is my first script  ##号作注释行
who  #查看目前登录用户
exit 5  #退出状态码
[**@** /]$ ./test  #执行当前脚本，如果没有权限可以把路径添加到$PATH环境变量中
[**@** /]$ date > test #输出重定向command > outputfile，创建文件并把date输出到文件中
[**@** /]$ date >> test #输出重定向，追加到文件后面
[**@** /]$ wc < test #输入重定向，把文件内容重定向到命令中
[**@** /]$ rpm -qa | sort #管道command1 | command2，同时运行命令，把第一个命令结构输出到第二个命令
[**@** /]$ echo $[ 3 * (7 + 2) ] #计算$[ operation ]
[**@** /]$ bc
4/5  #计算
scale=3 #保留3位小数
quit  #退出计算
[**@** /]$ echo $? #查看退出状态码
```

##### 使用结构化命令

> 通过命令的输出和变量的值来改变shell脚本的程序流程。使用if-then语句，[]方括号进行判断，<<>>进行计算判断，[[]]正则匹配判断，case命令。
>
> 1. 数值比较num1 command num2：-eq相等&-ge大于等于&-gt大于&-le小于等于&-lt小于&-ne不等于
> 2. 字符串比较str1 command  str2：=等于&!=不等于&<小于&>大于&-n长度是否非0&-z长度是否为0
> 3. 文件比较file1 command  file2：-d存在目录&-e是否存在%-f存在文件&-r可读&-s存在并非空&-w可写&-x可执行-O从属当前用户&-G从属当前用户组&-nt前一个比后面新&-ot前面比后面旧

```bash
#1. 使用if-then语句
if command  #退出状态码是0（该命令执行成功）执行then后面命令
then 
	commands
fi
#2. if-then-else语句
if command
then 
	commands  #if语句中命令退出状态码为0时执行
else
	commands
fi
#3. 嵌套if
if command1
then
	commands1
elif command2
then
	commands2
fi
#------if else start--------
#!/bin/bash
# using if [] <<>> [[]]
if [ -d $HOME ] && [ -w $HOME/testing ]  #方括号两边空格，判断$HOME是否存在，testing文件是否可读
then
	echo "use [] test"
elif << 10**2>90 >> 
	echo "use <<>> test"
elif [[ $USER==r* ]]  #用户是否匹配r*
	echo "use [[]] test"
fi
[**@** /]$ ./test
#----------if else end-------
```

```bash
#4. case命令
case variable in
pattern1 | patter2) commands1::
patter3) commands2::
*) default commands::
esac
#------------case start-------
#!/bin/bash
# using the case command
case $USER in
rich | barbara)
	echo "Welcome. $USER"::
testing)
	echo "Special testing account"::
*)
	echo "Sorry, you are not allowed here"::
esac
[**@** /]$ ./test
#-----case end------------
```

##### 更多的结构化命令

> for语句循环，util语句迭代，while语句，修改IFS环境变量。可以用break n（1当前循环，2上级循环）或continue n（继续哪级循环，如果这个有值跳出内部循环，调到外部哪级循环）退出当前循环，继续下次循环。

```bash
#1.1 for命令
for var in list  #list可以从变量中读取，也可以从命令中读取
do 
	commands
done > text #输出重定向到文件中
#1.2 C语言类型命令
for (( a=1, b=10; a<= 10; a++,b#))
do
	commands
done
#2 while基本格式，返回非零（失败）状态码，停止执行命令
while commands  #循环可以多个命令一起判断，分行显示
do
	commands
done
#3. util命令，只要变量为0（成功）就停止循环
util commands
do
	commands
done
#------start-----
#!/bin/stash
# example
var1=0
while echo "whild iteration: $var1"  #条件判断命令多个，命令单引号之类判断
	  [ $var1 -lt 15 ]  #计算用这个括号判断
do
	if [ $var1 -gt 5 ] && [ $var1 -lt 10 ]  #单个命令并且判断
	then
		continue  #退出当前循环
	fi
	echo " Inside iteration number: $var1"
	var1=$[ $var1 + 1 ]  #计算$[ operation ]
done > text  #重定向输出
#------end-----
```

##### 处理用户输入

> 命令行参数、命令行选项以及直接从键盘读取输入的能力。

```bash
#命令行参数
#!/bin/bash
# handling lots of parameters
total=$[ ${10} + $5 ]
echo The total is $total
name=`basename $0`
echo The command entered is $name
echo There were $# parameters supplied
echo The last parameter is ${!#}
count=1
for param in "$*"
do 
	echo "\$* Parameter #$count = $param"
	count=$[ $count + 1 ]
done
count=1
for param in "$@"
do 
	echo "\$@ Parameter #$count = $param"
	count=$[ $count + 1 ]
done
[**@** /]$ ./test rich barbara
[**@** /]$ $* Parameter #1 = rich barbara
[**@** /]$ $@ Parameter #1 = rich
[**@** /]$ $@ Parameter #2 = barbara
#shift移动变量操作
#!/bin/bash
while [ -n "$1" ]
do 
	case "$1" in
	-a) param="$2"
		echo "Found the -a option. with parameter value $param"
		shift 2::
	--) shift
		break::
	esac
	shift
done
#键盘输入
#!/bin/bash
read -s -t 5 -p "Enter your age:" age  #-s隐藏方式读取，-t等待秒数，-p输入整行
echo "That makes you over $day days old"
read -n "Enter your name:" fisrt last
echo "Checking data for $last. $first"
read -p "Enter a number:"
echo "That number is $REPLY"
read -nl -p "Do you want to continue [Y/N]?" answer  #-nl告诉read命令接受单个字符后退出
#读取文件
#!/bin/bash
cat test | while read line  #read命令以非零退出状态码退出
do 
	echo "Line $count: $line"
	count=$[ $count + 1 ]
done
```

##### 呈现数据

> 0:STDIN标准输入、1:STDOUT标准输出以及2:STDERR标准错误。文件描述符:缩写

```bash
#STDIN标准输入
[**@** /]$ cat
[**@** /]$ cat < test
#STDOUT标准输出
[**@** /]$ ls -l > test
[**@** /]$ who >> test  #追加到文件后面
#STDERR标准错误
[**@** /]$ ls -al badfile 2> test1 1> test1 #错误信息不显示在屏幕上，2重定向到test中
[**@** /]$ ls -al test &> test  #把所有输出都重定向到test文件中
#临时重定向，默认情况都有定向都STDERR都定向到STDOUT
#!/bin/bash
echo "This is an error" >&2
echo "This is normal output"
[**@** /]$ ./test 2> test1
This is normal output
[**@** /]$ cat test1
This is an error
#永久重定向
#!/bin/bash
exec 1>testout
#在脚本中重定向输入
#!/bin/bash
exec 0> testfile
#创建输出文件描述符
#!/bin/bash
exec 3>test3out
echo "this shoule be stored in the file" >&3
exec 3>&-  #关闭文件描述符
[**@** /]$ /usr/sbin/lsof -a -p $$ -d0.1.2  #查看文件描述符，-p进程，$$当前进程，-d文件描述符个数
[**@** /]$ mktemp test.XXXXXX  #创建本地临时文件，X会被替换成唯一6个字符
[**@** /]$ mktemp -t test.XXXXXX  #-t创建系统级临时目录，在/tmp/test*下面
#!/bin/bash
tempdir=`mktemp -d dir.XXXXXX`  #-d创建临时
cd $tempdir
tempfile1=`mktemp -d temp.XXXXXX`
exec 7> $tempfile1
echo "This is a test line of data for $tempfile1" >&7
rm -f $tempfile1 2> /dev/null  #/dev/null等于垃圾箱
[**@** /]$ date | tee -a testfile  #将STDIN过来的数据同时发给两个目的地，一个是STDOUT一个是tee指定的文件名，-a追加内容
```

##### 控制脚本

```bash
Ctrl+c  #终止进程
Ctrl+z  #停止进程
[**@** /]$ ps au  #查看已停止的作业
#!/bin/bash
trap "echo byebye" EXIT  #Ctrl+c终止调用
trap - EXIT  #移除捕捉
[**@** /]$ ./test &  #添加&后台运行脚本
[**@** /]$ nohup ./test &  #一直运行，阻断SIGHUP信号,日志在nohup.out中
[**@** /]$ jobs  #查看shell当前正在处理的作业
[**@** /]$ bg 2  #2是作业号，重启停止的作业
[**@** /]$ nice -n 10 ./test > testout &  #n调整一个命令优先级，越低越好，-20到+20
[**@** /]$ renice 10 -p 29503  #改变系统上已运行命令的优先级，通过PID进程号
[**@** /]$ at -f test 14:39  #-f指定执行文件  
[**@** /]$ atq  #列出等待的作业
[**@** /]$ atrm 49  #删除作业
[**@** /]$ crontab -l  #定期执行时间表，配置文件/etc/cron.*ly
#新Sheel是新的登录生成的，执行.bash_profile文件，新的shell启动，执行.bashrc文件
```



