##### 创建函数

> 普通函数两种定义方式，函数里面局部变量声明方式，数组传入和返回方式，递归函数实现，函数公用库实现方式。

```bash
#定义函数
#!/bin/bash
# using a function in a script
function func1{
  local temp=$[ $1 + $2 ]  #局部变量
  echo $temp
}
func2(){
  echo "This is an example of a function"
  read -p "Enter a value:" value
  echo $[ $value * 2 ]
}
value=`func1 10 15`  #传入参数
echo $value
result=`func2`  #echo的值方式获取函数返回值
echo "The new value is $result"
#数组变量和返回数组
#!/bin/bash
function testit {
  local newarray
  newarray=(`echo "$@"`)  #重组函数参数为数组
  echo "The new array value is: ${newarray[*]}"
  echo ${newarray[*]}  #返回数组
}
myarray={1 2 3 4 5}
echo "The original array is ${myarray[*]}"
arg1=`echo ${myarray[*]}`  #分解函数参数传入
result=(`testit $arg1`)
echo "The new array is: ${result[*]}"  #输出返回数组
#函数递归
#!/bin/bash
function factorial{
  if[ $1 -eq 1 ]
  then
  	echo 1
  else
  	local temp=$[ $1 - 1 ]
  	local result=`factorial $temp`
  	echo $[ $result * $1 ]
  fi
}
read -p "Enter value:" value
result=`factorial $value`
echo "The factorial of $value is: $result"
#函数库，需要和/bin/bash同一个目录
$ cat myfuncs  #两个$符号中定义公用库文件
# my script functions
function addem {
  echo $[ $1 + $2 ]
}
$
#!/bin/bash
# using functions defined in a library file
. ./myfuncs  #第一个.等于source命令
value1=10
value2=5
result1=`addem $value1 $value2`
echo "The result of adding them is: $result1"
#.bashrc文件中定义函数
```

##### 初识sed和gawk

> sed编辑器称为流编辑器，一行一行匹配处理STDIN，然后输出到STDOUT，并不修改原始数据。gawk更高级，允许修改和重新组织文件中的数据。

```bash
#sed options script file  -e多个script -f附件命令 -n不生成输出，等待print
[**@** /]$ echo "This is a test" | sed 's/test/big test/'  #s命令，big test替换test,只替换每行第一个匹配值
[**@** /]$ sed -e 's/brown/green/; s/dog/cat/' data1  #多个命令添加-e选项，用分号包围，两边不能有空格
[**@** /]$ sed 's/dog/cat/' data1  #修改data1，dog替换cat
[**@** /]$ cat script1
s/brown/green/
s/dog/cat/
[**@** /]$ sed -f script1 data1  #执行文件命令
#gawk options program file
[**@** /]$ gawk -F: '{print $1}' /etc/passwd  #读取用冒号分割的第一段
[**@** /]$ gawk 'BEGIN { print "The data4 File Contents:" }{ print $0 } END { print "End of File" }' data4  #BEGIN在正常程序脚本中处理数据，END打印完文件内容后执行END脚本中的命令
#sed编辑器基础
[**@** /]$ sed 's/test/trial/g' data5  #s/pattern/replacement/flags四种替换标记①数字②g全部③p打印修改后的行④w file保存修改后的行
[**@** /]$ sed 's!/bin/bash!/bin/csh!' /etc/passwd  #!作为字符串分隔符
[**@** /]$ sed '2.$s/dog/cat/' data1  #从第二行到最后替换
[**@** /]$ sed '3.$d' data  #从第三行开始删除
[**@** /]$ sed '/number 1/d' data  #删除匹配行
[**@** /]$ echo "Test Line 2" | sed 'i\Test Line 1'  #i插入一行在后面行之前
[**@** /]$ echo "Test Line 2" | sed 'a\Test Line 1'  #i插入一行在后面行之后
[**@** /]$ sed '3i\This is an inserted line' data  #在第3行之前插入行
[**@** /]$ sed '3c\This is a changed line of test' data  #修改第三行
[**@** /]$ sed 'y/123/789/' data  #[address]y/inchars/outchars转换命令处理单个字符
[**@** /]$ sed -n '/number 3/p' data  #-n禁止其他行，只打印匹配行
[**@** /]$ sed -n '/IN/w INcustomers' data  #匹配行写入文件INcustomers中
[**@** /]$ sed '3r data1' data2  #把data1中信息插入data2第3行之后
```

##### 正则表达式

> 特殊字符：.*[]^\+?|()，用\进行转义。
>
> 模式字符：`^`脱字符行首；`$`行位；`.`匹配任意单字符；字符组`[ab]`，匹配其中一个字符；排除字符组`[^ab]`，匹配除了ab之外任一字符；使用区间`[a-ch-m]`匹配a到c或h到m中任一字符；星号`*` 在字符或字符组后面匹配出现0次或多次；问好`?`类似星号，表明前面字符或字符组出现0次或1次；加号`+`类似星号，表明签名字符出现一次货多次；花括号`{m, n}`匹配字符集出现m到n次；聚合表达式`()`当成标准字符。

```bash
[**@** /]$ sed -n '/^this is a test$/p' data  #^脱字符行首，$行位
[**@** /]$ echo "He has a hat." | gawk '/[ch]at|dog/{print $0}'  #管道符号|类似OR用法
[**@** /]$ echo "tac" | gawk '/(c|b)a(b|t)/{print $0}'  #使用聚合表达式和管道符号
^\(?[2-9][0-9]{2}\)?(| |-|\.)[0-9]{3}( |-|\.)[0-9]{4}$  #匹配电话号码
^([a-zA-Z0-9_\-\.\+])@([a-zA-Z0-9_\-\.]+)\.([a-zA-Z]{2,5})$  #匹配邮箱地址
```

#####  sed进阶

> 模式空间是一块活动缓冲区，在sed编辑器执行命令时会保存sed编辑器要检验的文本。保持空间，在处理模式空间中其他行时用保持空间来临时保存一些行。

```bash
#多行命令,N将下一行和发现第一个单词的那行合并，D删除多行组中一行，P打印多行组中一行
[**@** /]$ sed '/first/{ N ; s/\n/ / }' data  #查找到first然后合并下一行，替换换行为空字符
[**@** /]$ sed '/^$/{ N; /header/D}' data  #查看空白行，然后合并下一行，匹配header删除多行模式空间第一行
[**@** /]$ sed -n 'N ; /System\nAdministrator/P' data  #打印匹配模式空间中第一行
#保持空间，h模式复制到保持，H模式附加到保持，g保持复制到模式，G保持附加到模式，x交换
#排除命令!让原本会起作用的命令不起作用
#跳转
[**@** /]$ sed '{2,3b ; s/This is/Is this/; s/bine./test?/' data  #2和3行跳过这个写明
[**@** /]$ sed '/s/.at/"&"/g'  #添加&可以用正则表达式匹配
```

##### gawk进阶

> 1. 内建变量，FIELDWIDTHS通过字数分割输出，FS输入字段分隔符，RS输入数据行分隔符，OFS输出字段分隔符，ORS输出数据行分隔符

```bash
[**@** /]$ gawk 'BEGIN{FS="."; OFS="-"}{print $1,$2,$3}' data  #打印信息
[**@** /]$ gawk 'BEGIN{FIELDWIDTHS="3 5 2 5"}{print $1,$2,$3,$4}'  data  #输出100 5.324 75 96.37
#自定义变量1
[**@** /]$ gawk 'BEGIN{x=4; x= x * 2 + 3; print x}'  #自定义变量x
#自定义变量2
[**@** /]$ cat script
BEGIN{print "The starting value is".n;FS=","}  #.号用来连接字符串
{print $n}
[**@** /]$ gawk -v n=3 -f script data  #打印出第3段数据
#数组变量var[index]=element例如capital["Ohio"="Columbus"]
#遍历数组变量
for(var in array){
  statements
}
#删除数组变量delete array[index]例如delete var["g"]
[**@** /]$ gark 'BEGIN{
> var["a"]=1
> for(test in var){
> print "Index:".test." - Value:".var[test]
> }
> delete var["a"]
> }'
Index:a - Value: 1
```

> 结构化命令

```bash
# if(condition){ statements }; else{ statements }
# while(condition){ statements }
# do{ statements }while(condition) 至少执行一次
# for( variable assignment: condition: iteration process)
[**@** /]$ gawk '{
> total=0  #自定义变量
> for(i=1; i<4; i++){  #结构化命令
>   total+=$i
>  }
> printf total 
> }
```

> 字符串函数，`index(s,t)`，没找到返回0；`length([s])`返回长度；`match(s,r[,a])`返回字符串s中正则表达式r的位置，存入a数组中；`split(s,a[,r])`将s用r分组；`substr[s,i[,n]]`截取字符串s，从i到n；`tolower(s)`转小写；`toupper(s)`转大写。