
## 一.Shell简介

>Shell是一个命令行解释器,接受应用程序/用户命令,然后调用操作系统内核

![[Attachments/Images/Pasted image 20231122155031.png]]
Linux提供了许多shell解析器 :

![[Attachments/Images/Pasted image 20231122155119.png]]

**sh和bash的关系 :**

1. Bourne Shell (`sh`)： 是Unix操作系统原始的Shell，其语法简单，但功能较基本
2. Bash（Bourne Again Shell）： 是Bourne Shell的扩展，具有更多的功能和改进，是许多Linux系统的默认Shell

`bash` 和 `sh` 是两种不同的 Shell，但由于 Bash 具有对 Bourne Shell 的兼容性，因此在许多情况下可以将 `bash` 视为 `sh` 的扩展

## 二.入门

>创建一个helloworld.sh

```shell
#!/bin/bash
echo "hello world"
```

`#!/bin/bash`是shell的开头,指定了shell脚本的解析器

**执行方式 :**

1. bash||sh 绝对路径||相对路径 , 不需要文件x权限 , 本质执行者为bash

![[Attachments/Images/Pasted image 20231122161502.png]]

2. 绝对路径||相对路径 , 需要文件x权限 , 本质执行者为当前用户

![[Attachments/Images/Pasted image 20231122161630.png]]

3. . + 路径 或者 source + 路径

## 三.变量

>显示当前Shell的所有变量 : set

### 1.系统预定义变量

常用系统变量 : $HOME , $PWD , $SHELL , $USER

### 2.自定义变量

**语法 :** 
1. name=value : 定义
2. unset name : 撤销
3. readonly name : 声明静态变量,不能unset
4. export name : 把变量提升为全局环境变量,可供其他shell程序使用

注意 : 
1. 变量默认是字符串类型无法进行数值运算
2. 如果有空格需要用引号

### 3.特殊变量

#### 3.1$n

**注明 :**

1. $0表示该脚本名称
2. $1-9代表第一到九个参数,十以上的参数需要使用\${10}
3. 脚本的参数紧跟在执行脚本的命令后面

#### 3.2$\#

获取所有输入参数个数

#### 3.3$*和\$@

1. \$\*代表命令行中所有的参数,但是把所有的参数看成一个整体
2. $@也代表所有的参数,但是把每个参数区分对待

![[Attachments/Images/Pasted image 20231124161142.png]]
![[Attachments/Images/Pasted image 20231124161157.png]]

#### 3.4$?

返回最后一次执行的脚本的返回状态,如果为0代表上一个脚本正确执行,如果非0则执行失败


## 四.运算符

基本语法 : `$(( 运算式 ))` 或 `$\[ 运算式 ]`

![[Attachments/Images/Pasted image 20231124154626.png]]

## 五.条件判断

### 1.基本语法

1. test condition
2. \[ condition ] : 前后空格

![[Attachments/Images/Pasted image 20231124154931.png]]

### 2.常用的判断条件

1. 两个整数之间比较 : 
	1. -eq 等于（equal）
	2. -ne 不等于（not equal）
	3. -lt 小于（less than）
	4. -le 小于等于（less equal）
	5. -gt 大于（greater than）
	6. -ge 大于等于（greater equal）
2. 字符串之间比较 :
	1. =或者\==判断相等
	2. !=判断不等
3. 文件权限判断 : 
	1. -r 读
	2. -w 写
	3. -x 执行
4. 文件类型判断 :
	1. -e 文件存在
	2. -f 存在且是常规的文件
	3. -d 存在且是一个目录

![[Attachments/Images/Pasted image 20231124155646.png]]

5. 多条件判断 :
	1. &&表示前一条命令执行成功时才会执行后一条命令
	2. ||表示上一条命令执行失败后才执行下一条命令

## 六.流程控制

### 1.if

基本语法 :

1. 单分支

```shell
if [ 条件判断式 ] ; then 
	程序 
fi
#if后面有空格
#或者
if [ 条件判断式 ] then 
	程序 
fi
```

2. 多分支

```shell
if [ 条件判断式 ] then 
	程序 
elif [ 条件判断式 ] then 
	程序 
else 
	程序 
fi
```

### 2.case

基本语法 :

```shell
case $变量名 in 
"值 1"） 如果变量的值等于值 1，则执行程序 1 
;; 
"值 2"） 如果变量的值等于值 2，则执行程序 2 
;; 
…省略其他分支… 
*） 
如果变量的值都不是以上的值，则执行此程序 
;; 
esac
```

注意事项 :

1. case 行尾必须为单词“in”，每一个模式匹配必须以右括号“）”结束
2. 双分号“;;”表示命令序列结束，相当于 java 中的 break
3. 最后的“\*）”表示默认模式，相当于 java 中的 default

### 3.for

基本语法 :

```shell
for (( 初始值;循环控制条件;变量变化 )) 
do 
	程序 
done
#或者
for 变量 in 值 1 值 2 值 3… 
do 
	程序 
done
```

### 4.while

基本语法 :

```shell
while [ 条件判断 ]
do
	程序
done
```

## 七.read读取控制台输入

基本语法 : `read [选项] [参数]`

选项 : 
	-p : 指定读取值时的提示符
	-t : 指定读取等待时间,默认一直等待

参数 :
	变量 : 指定读取值的变量名

![[Attachments/Images/Pasted image 20231124161632.png]]

## 八.函数

### 1.系统函数

1. `basename pathname [suffix] `
	删除 pathname中的所有的前缀包括最后一个'/'字符,然后将字符串显示出来 , 如果指定了suffix,会将提取的字符串再删去这个后缀

![[Attachments/Images/Pasted image 20231124163118.png]]

2. `dirname pathname`
	从给出的路径中去除文件名,返回剩下的路径

![[Attachments/Images/Pasted image 20231124163244.png]]

### 2.自定义函数

基本语法 :

```shell
[function] name[()]
{
	Action;
	[return int;]
}
```

>必须要在调用函数之前定义函数
>函数返回值只能通过$?获得,如果不加return返回最后一条命令运行结果,return后跟0-255

![[Attachments/Images/Pasted image 20231124164606.png]]

## 九.文本处理工具

### 1.cut

基本用法 : `cur [选项] filename`

选项 :
	-f : 列号,提取第几列
	-d : 分隔符,按照指定分隔符分割列,默认为制表符'\t'
	-c : 按照字符进行切割,后面加n表示去第几列

![[Attachments/Images/Pasted image 20231124165818.png]]

取出第二列 : 

![[Attachments/Images/Pasted image 20231124165839.png]]

取出第三列字符 :

![[Attachments/Images/Pasted image 20231124170000.png]]

### 2.awk

基本用法 : `awk [选项] '/pattern1 {action1} /pattern2 {action2} ...' filename`

选项 :
	-F : 指定输入文件分隔符
	-v : 赋值一个用户定义变量


例 :
`awk -F : '/^root/{print $1","$7}' passwd` : 以:为分隔符,输出以root开头的行的第1列和第7列
`awk -F : 'BEGIN{print "user, shell"} {print $1","$7} END{print "dahaige,/bin/zuishuai"}' passwd` : 只显示/etc/passwd 的第一列和第七列，以逗号分割，且在所有行前面添加列名 user， shell 在最后一行添加"dahaige，/bin/zuishuai"
`awk -v i=1 -F : '{print $3+i}' passwd` : 将 passwd 文件中的用户 id 增加数值 1 并输出

内置变量 :
	FILENAME : 文件名
	NR : 已读的记录数（行号）
	NF : 浏览记录的域的个数（切割后，列的个数）





