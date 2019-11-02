### Shell

##### 查看当前系统shell版本： ` echo $SHELL` 

#### 执行shell脚本： 

1. ` /bin/bash 文件路径/。。.sh ` 
2. ` . jiaoben.sh` #脚本在当前目录下
3. ` source jioaben.sh` 

#### 变量

**普通变量**

**命令变量** 

- 变量名 = ```命令`  `` (非单引号)

- 变量名 = $(命令)

##### 全局变量

**env** 查看全局变量

**定义全局变量** 

` export 变量 = 值` （推荐）

>  变量 = 值
>
> ` export 变量` 

##### 查看变量

```latex
$ “变量”
$ {变量}
$ 变量

```

##### 取消变量(删除)

` unset 变量名` 

##### 命令

```latex
$n 获取第n个参数
$# 获取传参总数量 
```

```latex
$?
0   执行成功（上一条指令）
非0 执行失败
```

##### 表达式



``` latex
1. [ 1 =2 ] # []两端有空格
2. test 1=2
```

##### diff 对比文件差异

>  diff   file1 file2可以对比两个文件file1和file2的差异。一般在对比时是以第二个文件为标准的。比如说2d1的意思是第一个文件第二行删除linux就会和第二个文件内容一致。1a2意思是说第一个文件添加linux内容就会与第二个文件内容一致。还会有比如2c2意思是第一个文件第二行改变内容就会和第二个文件的内容一致。
> ------------------------------------------------

![img](C:\Users\Administrator\Desktop\md分享\Myself_markdownNote\201806141601138)

  diff 指令也可以用来对比两个**目录**的内容： 

![img](C:\Users\Administrator\Desktop\md分享\Myself_markdownNote\image\20180614161242275)

#####   sort和uniq的使用： 

  统计文件中每个数字出现的个数：  左边为数字个数，右边为排序后的数字 ![img](C:\Users\Administrator\Desktop\md分享\Myself_markdownNote\image\20180614161734504)

 只显示唯一的数字： ![img](C:\Users\Administrator\Desktop\md分享\Myself_markdownNote\image\20180614161908335)

 只显示重复的数字： ![img](C:\Users\Administrator\Desktop\md分享\Myself_markdownNote\image\20180614161936303)

 **3. test指令：**

**![img](C:\Users\Administrator\Desktop\md分享\Myself_markdownNote\image\2018061416211337)** 

![img](C:\Users\Administrator\Desktop\md分享\Myself_markdownNote\image\20180614162134822)

> 举例：test命令书写脚本比较数的大小

           其中-z用来判断是否从键盘收了数据$1,&&指前面的条件为真，就执行后面的指令。在第一行指令的意思是如果没有从键盘接收到$1数据，就执行后面的指令。与&&相对立的指令为||，意思是如果前面的条件为假，就执行||后面的指令：
> ------------------------------------------------

![img](C:\Users\Administrator\Desktop\md分享\Myself_markdownNote\image\20180614162304862)

![img](C:\Users\Administrator\Desktop\md分享\Myself_markdownNote\image\20180614162727711)

 **4.  tr指令：用来进行大小写字母的转换：** 

>  举例：将从键盘接收的字母，以不区分大小写的方式判断其是否为hello.如果是，输出yes，如果不是，输出no。指令tr 'A-z'  'a-z'就是用来完成大写字母到小写字母的转换： 

![img](C:\Users\Administrator\Desktop\md分享\Myself_markdownNote\image\20180614163538393)

![img](C:\Users\Administrator\Desktop\md分享\Myself_markdownNote\image\20180614163743140)

##### if语句

```shell
if [ "$#" -eq 1 ]
then
	echo
elif []
then
	echo
else
	echo
fi
```

##### case语句

```shell
a = "$1"
case "${a}" in 
值1)
	echo
	;;
值2)
	echo
	;;
*)
	echo
	;;
esac
```

##### for 循环

` 若shell语句存在$()` 

 `先执行()里面的语句 `

```shell
for i in $(ls /root)
do
	echo
done
```

##### while 语句

```shell
a = 1
while [ "${a} -lt 5" ] 
do
 	echo "${a}"
	a=$((a+1))   #(())计算表达式
done
```

##### until 语句

` 条件满足,退出` 

```shell
a = 1
until [ "${a} -eq 5" ] 
do
 	echo "${a}"
	a=$((a+1))   
done
```

##### 函数

```shell
#定义函数/ 函数名自定义(dayin)
dayin(){
	echo "$1"  #获取函数的参数
}
#调用函数
dayin "$1"   #获取脚本的参数
```









