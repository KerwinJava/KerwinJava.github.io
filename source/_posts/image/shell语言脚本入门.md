---
title: shell语言脚本入门
date: 2024-02-21 16:55:09
tags: 
- shell
- linux
categories:
- shell
- linux脚本


---

公司的微服务终于运行起来了，为了简化运维人员的工作量，又接下了一个大活，为部署提供一套可行的快速解决方案，这其中不可避免的会接触到很多脚本的编写，正好借着这个机会，可以把shell脚本系统的学习一下。

#### shell语言

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

Shell 语言通常指的是用于与操作系统交互的脚本语言，这些脚本语言被设计为与 Shell 解释器一起工作。Shell 是一种命令行界面，它允许用户输入命令来执行程序、管理文件系统、执行任务自动化等。
Shell 脚本是一种脚本语言，它使用 Shell 的命令和功能来执行任务。Shell 脚本通常以 .sh 扩展名保存，例如 script.sh。要运行这样的脚本，你需要有执行权限，并且可以使用以下命令：

```bash
bash script.sh
```

或者，如果你直接运行脚本，如果它有执行权限，你可以使用：

```
./script.sh

```

Shell 编程跟 JavaScript、php 编程一样，只要有一个能编写代码的文本编辑器和一个能解释执行的脚本解释器就可以了。

常见的解释器有：

- Bourne Shell（/usr/bin/sh或/bin/sh）

- Bourne Again Shell（/bin/bash）

- C Shell（/usr/bin/csh）

- K Shell（/usr/bin/ksh）

- Shell for Root（/sbin/sh）

   Bash，也就是 Bourne Again Shell，由于易用和免费，Bash 在日常工作中被广泛使用。同时，Bash 也是大多数Linux 系统默认的 Shell。

  

  1. 新建一个脚本

     ```bash
     touch test.sh
     ```

     

  2. 编辑脚本，使用vi/vim打开上面新建的文件

     ```bash
     #!/bin/bash   #指定解释器
     echo "Hello World !"  #echo 命令用于向窗口输出文本。
     ```

     

  3. 执行脚本

     ```
     chmod +x test.sh #给脚本文件添加可执行权限
      
     ./test.sh  #执行脚本文件
     ```

     

#### 变量

##### 变量赋值

```
text1="say it" #使用等号定义变量
```

##### 使用变量

```
echo $text1
echo ${text1} ok? #可以使用大括号 方便字符串拼接
```

##### 设置只读

```
readonly text1 #设置完成后 不可再对变量赋值 会提示“./test.sh: line 7: text1: readonly variable”
```

##### 删除变量

```
unset text1 #删除后 使用变量为空
```

##### 变量类型

> 变量默认被视为字符串。
> 除了字符串类型还有整数变量、数组变量、关联数组、环境变量、特殊变量

###### 整数变量

```
declare -i my_integer=42 #定义并且赋值一个整数变量
```

###### 数组变量

```
my_array=(1 2 3 4 5) #定义且赋值
echo 所有元素：${my_array[*]}
echo 第二个元素：${my_array[1]}
# 获取数组长度
length=${#my_array[@]}
echo "Number of array: $length"
```

###### 关联数组

```
declare -A map
map['name']="bob"
map['age']=18
map['city']="NanJing"

echo ${map["name"]}\'s age is ${map["age"]}
echo "数组map的元素为："${map[*]}
echo "数组map的元素个数为："${#map[*]}
echo "数组map键元素为："${!map[*]}
```

###### 环境变量

> 由操作系统或用户设置的特殊变量，用于配置 Shell 的行为和影响其执行环境

```
echo "path:"${PATH}
```

特殊变量

```
echo $0 #脚本名称
echo $? #表示上一个命令的退出状态，0表示没有错误，其他任何值表明有错误。
echo $$ #脚本运行的当前进程ID号。
echo $# #表示传递给脚本的参数数量
echo $n #表示执行脚本第n个参数
echo $* #将传递给脚本的所有参数合并为一个字符串输出
#比如 ./test.sh abc执行的话，echo $#返回1  echo $1返回abc
```



#### 算术运算符

##### 数学运算

> bash本身不支持数学运算，可以通过expr命令来实现
>
> 完整表达式需要使用反引号`包含
>
> 运算符两侧要用空格隔开
>
> 条件表达式要放在方括号[]之间，并且要有空格

```
declare -i a=60
declare -i b=30
val=`expr ${a} + ${b}`
echo "a+b="${val} #加法
echo "a/b="`expr ${a} / ${b}` #除法
echo "a*b="`expr ${a} * ${b}` #乘法
echo "a%b="`expr ${a} % ${b}` #取余
echo "a==60 is "`expr ${a} == 60` #是否相等
echo "a==59 is "`expr ${a} == 59`
echo "a!=59 is "`expr ${a} != 59` #是否不等于
if [ $a == 60 ]
then
    echo "a等于60"
fi
```

##### 关系运算

```
if [ $a -eq 60 ]
then
    echo -eq a等于60
fi
if [ $a -ne 60 ]
then
    echo -ne a不等于60
else
    echo "-ne: a等于60"
fi
if [ $a -gt 60 ]
then
    echo gt a大于60
else
    echo gt a不大于60
fi
if [ $a -lt 60 ]
then
    echo lt a小于60
else
    echo lt a不小于60
fi
if [ $a -ge 60 ]
then
    echo ge a大于等于60
else
    echo ge a小于60
fi
if [ $a -le 60 ]
then
    echo le a小于等于60
else
    echo le a大于60
fi
```

##### 布尔运算

> 用于if命令

```
if [ $a -ne 60 -o $b -eq 30 ]#或运算 有一个为真则为真
then
    echo -o a不等于60或者b等于30
fi
if [ $a -eq 60 -a $b -eq 30 ]#与运算 两个都为真才为真
then
    echo -o a等于60并且b等于30
fi
```

##### 逻辑运算符

> 用于连接命令
>
> && 逻辑的AND  [[ $a -ne 60 && $b -eq 30 ]] 返回false
>
> || 逻辑的OR [[ $a -ne 60 || $b -eq 30 ]] 返回true

##### 字符串运算符

```
c="string1"
d="string11"
e=""

if [ $c = string1 ]#是否相等
then
    echo = c = string1
fi
if [ $d = string1 ]
then
    echo = d = string1
else
    echo = d不等于string1
fi
if [ $d != string1 ]#是否不等
then
    echo != d 不等于 string1
else
    echo != d等于string1
fi
if [ -z $d ] #长度是否为0
then
    echo -z d的长度为0
else
    echo -z d的长度不为0
fi
if [ -z $e ]
then
    echo -z e的长度为0
else
    echo -z e的长度不为0
fi
if [ -n "$d" ] #长度是否不为0
then
    echo "-n d的长度不为0"
else
    echo "-n d的长度为0"
fi
if [ -n "$e" ]
then
    echo "-n e的长度不为0"
else
    echo "-n e的长度为0"
fi

if [ $d ] #是否为空
then
    echo "$ d不为空"
else
    echo "$ d为空"
fi
if [ $e ]
then
    echo "$ e不为空"
else
    echo "$ e为空"
```

##### 文件测试运算符

> 文件测试运算符用于检测Unix文件的各种属性

| -b file | 检测文件是否是块设备文件，如果是，则返回 true。              | [ -b $file ] 返回 false   |
| ------- | ------------------------------------------------------------ | ------------------------- |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true。            | [ -c $file ] 返回 false。 |
| -d file | 检测文件是否是目录，如果是，则返回 true。                    | [ -d $file ] 返回 false。 |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 | [ -f $file ] 返回 true。  |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true。            | [ -g $file ] 返回 false。 |
| -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。  | [ -k $file ] 返回 false。 |
| -p file | 检测文件是否是有名管道，如果是，则返回 true。                | [ -p $file ] 返回 false。 |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true。            | [ -u $file ] 返回 false。 |
| -r file | 检测文件是否可读，如果是，则返回 true。                      | [ -r $file ] 返回 true。  |
| -w file | 检测文件是否可写，如果是，则返回 true。                      | [ -w $file ] 返回 true。  |
| -x file | 检测文件是否可执行，如果是，则返回 true。                    | [ -x $file ] 返回 true。  |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true。     | [ -s $file ] 返回 true。  |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true。          | [ -e $file ] 返回 true。  |
| -S file | 判断某文件是否 socket。                                      |                           |
| -L file | 检测文件是否存在并且是一个符号链接                           |                           |

```
if [ -r /home/zxin10/test.sh ]
then
    echo "readable"
else
    echo "unreadable"
fi
```

#### echo命令

##### 显示普通字符串

```
echo "It is a test"
echo It is a test #与上面效果一致

echo "\"It is a test\"" #显示转义字符串

```

##### 显示变量

```
read name #从输入中读取赋值给name
echo "$name It is a test" #显示输入 +“It is a test”
```

##### 显示不换行

```
echo -e "OK! \c" # -e 开启转义 \c 不换行
echo "It is a test"
```

##### 显示结果定向至文件

```
echo "It is a test" > myfile
```

##### 原样输出字符串

> ##### 不进行转义或取变量(用单引号)

```
echo '$name\"'
```

##### 显示命令执行结果

> 使用反引号

```
echo `date`
```

#### 流程控制

##### if 

> 使用fi结尾

```
if condition
then
    command1 
    command2
    ...
    commandN 
fi
```

写成一行

```
if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi
```

##### if else

```
if condition
then
    command1 
    command2
    ...
    commandN
else
    command
fi
```

##### if else-if else

```
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

#### for循环

```
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

写成一行：

```
for var in item1 item2 ... itemN; do command1; command2… done;
```

#### while循环

##### 条件循环

```
while condition
do
    command
done
```

##### 无限循环

```
while :
do
    command
done
```

或者

```
while true
do
    command
done
```

或者

```
for (( ; ; ))
```

#### Until循环

> until 循环执行一系列命令直至条件为 true 时停止。与while相反。

```
until condition
do
    command
done
```

#### case ... esac

> 与其他语言中的 switch ... case 语句类似，是一种多分支选择结构，每个 case 分支用右圆括号开始，用两个分号 **;;** 表示 break，即执行结束，跳出整个 case ... esac 语句，esac（就是 case 反过来）作为结束标记。

```
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2)
    command1
    command2
    ...
    commandN
    ;;
esac
```

```
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
  1) echo '你选择了 1'
  ;;
  2) echo '你选择了 2'
  ;;
  3) echo '你选择了 3'
  ;;
  4) echo '你选择了 4'
  ;;
  *) echo '你没有输入 1 到 4 之间的数字'
  ;;
esac
```

#### shell函数

##### 函数定义



```
[  function ] funname [()]

{

  action;

  [return int;]

}
```

- 可以带 **function fun()** 定义，也可以直接 **fun()** 定义,不带任何参数。

- 参数返回，可以显示加：**return** 返回，如果不加，将以最后一条命令运行结果，作为返回值。 **return** 后跟数值 **n(0-255)**.

  **无返回样例**

```
demoFun(){
  echo"这是我的第一个 shell 函数!"
}
echo "-----函数开始执行-----"
demoFun
echo "-----函数执行完毕-----"
```

**带return样例**

> 函数返回值在调用该函数后通过 **$?** 来获得。
>
> **return** 语句只能返回一个介于 0 到 255 之间的整数，而两个输入数字的和可能超过这个范围。

```
funWithReturn(){
  echo "这个函数会对输入的两个数字进行相加运算..."
  echo "输入第一个数字: "
  read aNum
  echo "输入第二个数字: "
  read anotherNum
  echo "两个数字分别为 $aNum 和 $anotherNum !"
  return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
```

##### 函数调用

> 调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数...

```
funWithParam(){
  echo "第一个参数为 $1 !"
  echo "第二个参数为 $2 !"
  echo "第十个参数为 $10 !"
  echo "第十个参数为 ${10} !"
  echo "第十一个参数为 ${11} !"
  echo "参数总数有 $# 个!"
  echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```

#### 输入输出重定向

| command > file  | 将输出重定向到 file。                              |
| --------------- | -------------------------------------------------- |
| command < file  | 将输入重定向到 file。                              |
| command >> file | 将输出以追加的方式重定向到 file。                  |
| n > file        | 将文件描述符为 n 的文件重定向到 file。             |
| n >> file       | 将文件描述符为 n 的文件以追加的方式重定向到 file。 |
| n >& m          | 将输出文件 m 和 n 合并。                           |
| n <& m          | 将输入文件 m 和 n 合并                             |
| << tag          | 将开始标记 tag 和结束标记 tag 之间的内容作为输入。 |

> 0 是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）



```
command1 > file1  #执行command1然后将输出的内容存入file1  file1内的已经存在的内容将被新内容替代
command1 < file1  #从文件file1获取输入
command 2>file  #stderr 重定向到 file
command > file 2>&1 #将 stdout 和 stderr 合并后重定向到 file 或者 command >> file 2>&1
command < file1 >file2  #将 stdin 重定向到 file1，将 stdout 重定向到 file2

command > /dev/null  #希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null

```

#### shell文件包含

> 和其他语言一样，Shell 也可以包含外部脚本。这样可以很方便的封装一些公用的代码作为一个独立的文件

```
. filename   # 注意点号(.)和文件名中间有一空格

或

source filename
```