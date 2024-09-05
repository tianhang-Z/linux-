## 概述

![image-20240306170006096](./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240306170006096.png)

🖋 Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

🖊 Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

🖌 Shell 脚本（shell script），是一种为 shell 编写的脚本程序。

🖍 Shell 编程跟 JavaScript、php 编程一样，只要有一个能编写代码的文本编辑器和一个能解释执行的脚本解释器就可以了。
📃 Linux 的 Shell 种类众多，常见的有：

Bourne Shell（/usr/bin/sh或/bin/sh）
Bourne Again Shell（/bin/bash）
C Shell（/usr/bin/csh）
K Shell（/usr/bin/ksh）
Shell for Root（/sbin/sh）
……

📃 sh/bash/csh/Tcsh/ksh/pdksh等shell的区别：

    👉 sh(全称 Bourne Shell)： 是UNIX最初使用的 shell，而且在每种 UNIX 上都可以使用。
    👉 Bourne Shell： 在 shell 编程方面相当优秀，但在处理与用户的交互方面做得不如其他几种 shell。
    👉 bash(全称 Bourne Again Shell)： LinuxOS 默认的，它是 Bourne Shell 的扩展。 与 Bourne Shell 完全兼容，并且在 Bourne Shell 的基础上增加了很多特性。可以提供命令补全，命令编辑和命令历史等功能。它还包含了很多 C Shell 和 Korn Shell 中的优点，有灵活和强大的编辑接口，同时又很友好的用户界面。
    👉 csh(全称 C Shell)： 是一种比 Bourne Shell更适合的变种 Shell，它的语法与 C 语言很相似。
    👉 Tcsh： 是 Linux 提供的 C Shell 的一个扩展版本。Tcsh 包括命令行编辑，可编程单词补全，拼写校正，历史命令替换，作业控制和类似 C 语言的语法，他不仅和 Bash Shell 提示符兼容，而且还提供比 Bash Shell 更多的提示符参数。
    👉 ksh(全称 Korn Shell)： 集合了 C Shell 和 Bourne Shell 的优点并且和 Bourne Shell 完全兼容。
    👉 pdksh： 是 Linux 系统提供的 ksh 的扩展。pdksh 支持人物控制，可以在命令行上挂起，后台执行，唤醒或终止程序。

📃ubuntu提供的 Shell 解析器：

<img src="./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240306170313308.png" alt="image-20240306170313308" style="zoom:80%;" />

==ubuntu的sh链接到了dash==

Ubuntu和debian 的 shell 默认安装的是 dash，而不是 bash。

dash 比 bash 更轻，更快。但 bash 却更常用。

通过以下方式可以使 shell 切换回 bash：
`$sudo dpkg-reconfigure dash` 之后选择否

也可以直接修改 /bin/sh 链接文件，将其指定到 /bin/bash：
`$sudo ln -s /bin/bash /bin/sh`

`ll /bin/ |grep dash`

<img src="./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240306170649688.png" alt="image-20240306170649688" style="zoom:80%;" />

`ls -l /bin/ |grep sh`

## 编写shell脚本
==脚本以 #!/bin/bash 开头（指定解析器）==

#! 告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序。

打开文本编辑器(可以使用 vi/vim 命令来创建文件)，新建一个文件 helloworld.sh，扩展名为 sh（sh代表shell），扩展名并不影响脚本执行。
```shell
[root@Demo shells]$ touch helloworld.sh
[root@Demo shells]$ vim helloworld.sh

// 在 helloworld.sh 中输入如下内容

#!/bin/bash
echo "helloworld"

```

`#!` 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell。

`echo` 命令用于向窗口输出文本。

## 脚本的常用执行方式
📝 采用 bash  sh dash + 脚本的相对路径或绝对路径（不用赋予脚本+x 权限）

📃 sh+脚本的相对路径

```shell
[root@Demo shells]$ sh ./helloworld.sh
Helloworld
[root@Demo shells]$ sh /home/zth/Desktop/helloworld.sh
helloworld
[root@Demo shells]$ bash ./helloworld.sh
Helloworld
[root@Demo shells]$ dash ./helloworld.sh
Helloworld
```


🖇 这种执行方法，本质是 bash 解析器帮你执行脚本，所以脚本本身不需要执权限。

==📝 采用直接输入脚本的绝对路径或相对路径执行脚本（必须具有可执行权限+x）==

<img src="./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240307192139508.png" alt="image-20240307192139508" style="zoom:80%;" />

```shell
[root@Demo shells]$ chmod +x helloworld.sh
[root@Demo shells]$ ./helloworld.sh
helloword
[root@Demo shells]$ /home/zth/Desktop/helloworld.sh
```

==🖇 这种执行方法，本质是脚本需要自己执行，所以需要执行权限。==

📃 分别用 sh，bash，./ 和 . 的方式来执行

```shell
$ sh hello.sh
$ dash hello.sh
$ ./hello.sh
$ . hello.sh
```

## 变量
### 常用系统变量

`$HOME、$PWD、$SHELL、$USER `

📃 查看系统变量的值

 `echo $HOME`
📃 显示当前 Shell 中所有变量  几种方法

```shell
set
env
printenv
printenv USER  #查看USER环境变量
```

### 变量规则与类型

#### 规则

==📍 定义变量时，变量名不加$符号==

📍 变量名和等号之间不能有空格

📍 变量名的命名须遵循规则：

    👉 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头。
    
    👉 中间不能有空格，可以使用下划线 _。
    
    👉 不能使用标点符号。
    
    👉 不能使用bash里的关键字（可用help命令查看保留关键字）。

📍 使用一个定义过的变量，只要在变量名前面加美元符号即可。

📍 变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界 (推荐给所有变量加上花括号)。

📍 已定义的变量，可以被重新定义。

#### 类型

📍 变量类型：运行shell时，会同时存在三种变量：

👉 ==局部变量==： 局部变量在脚本或命令中定义，==仅在当前shell实例中有效==，其他shell启动的程序不能访问局部变量。

👉 环境变量： 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。

👉 shell变量： shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行。

```shell
$my_var=hello
$set |grep my_var    #可以查到my_var
my_var=hello
$bash  #进入新的bash  
$set |grep my_var   #查不到my_var  
$exit
$export my_var          #变量声明为全局变量
$bash
$set |grep my_var
```

==子shell的全局变量无法被父shell访问，父shell声明的全局变量可以被子shell访问==

📍 只读变量：使用 readonly 命令可以将变量定义为只读变量，==只读变量的值不能被改变==。

📍 删除变量：使用 unset 命令可以删除变量。变量被删除后不能再次使用。==unset 命令不能删除只读变量==。

📃 参考实例

```shell
# 定义变量 A
[root@Demo shells]$ A=5
[root@Demo shells]$ echo $A

#给变量 A 重新赋值
[root@Demo shells]$ A=8
[root@Demo shells]$ echo $A

#撤销变量 A
[root@Demo shells]$ unset A
[root@Demo shells]$ echo $A

#声明静态的变量 B=2，不能 unset
[root@Demo shells]$ readonly B=2
[root@Demo shells]$ echo $B
2
[root@Demo shells]$ B=9
-bash: B: readonly variable 

#在 bash 中，变量默认类型都是字符串类型，无法直接进行数值运算
[root@Demo shells]$ C=1+2
[root@Demo shells]$ echo $C
1+2

#变量的值如果有空格，需要使用双引号或单引号括起来
[root@Demo shells]$ D=I love you
-bash: world: command not found
[root@Demo shells]$ D="I love you"
[root@Demo shells]$ echo $D
I love you


#可把变量提升为全局环境变量，可供其他 Shell 程序使用
#在 helloworld.sh 文件中增加 echo $B
[root@Demo shells]$ vim helloworld.sh
#!/bin/bash
echo "helloworld"
echo $B
[root@Demo shells]$ ./helloworld.sh
Helloworld
#发现并没有打印输出变量 B 的值
#使用 export 变量名   则打印出了B的值9
[root@Demo shells]$ export B
[root@Demo shells]$ ./helloworld.sh
helloworld
9
```

### 特殊变量-参数传递

==给脚本输入参数==

#### $n

![image-20240307203640403](./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240307203640403.png)

![image-20240307204834740](./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240307204834740.png)

```
$ ./hello.sh 1st 2nd
```

![image-20240307204853821](./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240307204853821.png)

```
$#	传递到脚本的参数个数
$$	脚本运行的当前进程ID号
$!	后台运行的最后一个进程的ID号
$-	显示Shell使用的当前选项，与set命令功能相同。
$?	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。
```

#### $* 

以一个单字符串显示所有向脚本传递的参数。如"$*“用「”」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。

#### $@ 

  👉 * 和@都是引用所有参数。

  👉 只有在双引号中体现出来。假设在脚本运行时写了三个参数 1、2、3，，则 " * " 等价于 “1 2 3”（传递了一个参数），而 “@” 等价于 “1” “2” “3”（传递了三个参数）。

```shell
echo "-- \$* 演示 ---"
for i in "$*"; do
    echo $i
done

echo "-- \$@ 演示 ---"
for i in "$@"; do
    echo $i
done

$ chmod +x test.sh 
$ ./test.sh 1 2 3
-- $* 演示 ---
1 2 3
-- $@ 演示 ---
1
2
3
```

#### $?

$?	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。

## Shell 注释

以 # 开头的行就是注释，会被解释器忽略，通过每一行加一个 # 号设置多行注释。

```shell
#这是一个注释
#
##### 开始
# 
#
这里可以添加描述信息
#
#
##### 结束  #####
```

```shell
# 多行注释还可以使用以下格式

:<<EOF
注释内容...
注释内容...
注释内容...
EOF
```


EOF 也可以使用其他符号

```
:<<'
注释内容...
注释内容...
注释内容...
'

:<<!
注释内容...
注释内容...
注释内容...
!
```



## shell字符串

字符串是shell编程中最常用最有用的数据类型，字符串可以用单引号，也可以用双引号，也可以不用引号。

### 单引号

    👉 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
    
    👉 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。

### 双引号

    👉 双引号里可以有变量。
    
    👉 双引号里可以出现转义字符。

#### 📃 拼接字符串

```shell
your_name="runoob"

# 使用双引号拼接,输出结果为：hello, runoob ! hello, runoob !

greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting  $greeting_1

# 使用单引号拼接,输出结果为：hello, runoob ! hello, ${your_name} !

greeting_2='hello, '$your_name' !'
greeting_3='hello, ${your_name} !'
echo $greeting_2  $greeting_3
```

#### 📃 获取字符串长度

```shell
string="abcd"
echo ${#string}   # 输出 4

#变量为数组时，${#string} 等价于 ${#string[0]}:

string="abcd"
echo ${#string[0]}   # 输出 4

#计算字符长度也可是使用 length。

string="hello,everyone my name is mingming"  
# string字符串里边有空格,所以需要添加双引号
expr length "$string"   # 输出:34

#使用 expr 命令时，表达式中的运算符左右必须包含空格，如果不包含空格，将会输出表达式本身
expr 5+6     # 直接输出 5+6
expr 5 + 6   # 输出 11

#对于某些运算符，还需要我们使用符号"\"进行转义，否则就会提示语法错误
expr 5 * 6       # 输出错误
expr 5 \* 6      # 输出30
```

#### 📃 提取子字符串

```
#以下实例从字符串第 2 个字符开始截取 4 个字符

string="runoob is a great site"
echo ${string:1:4}   # 输出 unoo
```


#### 📃 查找子字符串

```
查找字符 i 或 o 的位置(哪个字母先出现就计算哪个)
string="runoob is a great site"
echo `expr index "$string" io`  # 输出 4
```

#### 📃 字符串截取

```shell
0️⃣ 定义字符串
var=http://www.aaa.com/123.htm

1️⃣ # 号截取，删除左边字符，保留右边字符
echo ${var#*//} # 结果是 ：www.aaa.com/123.htm
   📎 # 其中 var 是变量名，# 号是运算符，*// 表示从左边开始删除第一个 // 号及左边的所有字符，即删除 http://

2️⃣## 号截取，删除左边字符，保留右边字符
echo ${var##*/} # 结果是 123.htm
   📎 ##*/ 表示从左边开始删除最后（最右边）一个 / 号及左边的所有字符，即删除 http://www.aaa.com/

3️⃣ %号截取，删除右边字符，保留左边字符
echo ${var%/*} # 结果是：http://www.aaa.com
   📎 %/* 表示从右边开始，删除第一个 / 号及右边的字符

4️⃣ %% 号截取，删除右边字符，保留左边字符
echo ${var%%/*} # 结果是：http:
   📎 %%/* 表示从右边开始，删除最后（最左边）一个 / 号及右边的字符

5️⃣ 从左边第几个字符开始，及字符的个数
echo ${var:0:5} # 结果是：http:

   📎 其中的 0 表示左边第一个字符开始，5 表示字符的总个数。

6️⃣ 从左边第几个字符开始，一直到结束。
echo ${var:7} # 结果是 ：www.aaa.com/123.htm
   📎 其中的 7 表示左边第8个字符开始，一直到结束。

7️⃣ 从右边第几个字符开始，及字符的个数
echo ${var:0-7:3} # 结果是：123
   📎 其中的 0-7 表示右边算起第七个字符开始，3 表示字符的个数。

8️⃣ 从右边第几个字符开始，一直到结束。
echo ${var:0-7} # 结果是：123.htm
   📎 表示从右边第七个字符开始，一直到结束。
📍 左边的第一个字符是用 0 表示，右边的第一个字符用 0-1 表示。
```

    👉 #、## 表示从左边开始删除。一个 # 表示从左边删除到第一个指定的字符；两个 # 表示从左边删除到最后一个指定的字符。
    
    👉 %、%% 表示从右边开始删除。一个 % 表示从右边删除到第一个指定的字符；两个 % 表示从右边删除到最后一个指定的字符。
    
    👉 删除包括了指定的字符本身。

#### 📃 read 命令用于获取键盘输入信息

🔦 它的语法形式一般是：`read [-options] [variable...]`

```shell
-p 参数由于设置提示信息

read -p "input a val:" a    #获取键盘输入的 a 变量数字
read -p "input b val:" b    #获取键盘输入的 b 变量数字
r=$[a+b]                    #计算a+b的结果 赋值给r  不能有空格
echo "result = ${r}"        #输出显示结果 r
```

## Shell 数组
bash支持一维数组（不支持多维数组），并且没有限定数组的大小。

数组元素的下标由 0 开始编号。获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值应大于或等于 0。

在 Shell 中，用括号来表示数组，数组元素用"空格"符号分割开。定义数组的一般形式为： `数组名=(值1 值2 ... 值n)`

#### 初始化

📃 例如：

```shell
array_name=(value0 value1 value2 value3)

#或者
array_name=(
value0
value1
value2
value3
)

#还可以单独定义数组的各个分量。可以不使用连续的下标，而且下标的范围没有限制。
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen
```

#### 读取数组

读取数组元素值的一般格式是：`${数组名[下标]}`

📃 例如：

```SHELL
valuen=${array_name[n]}
#使用 @ 符号可以获取数组中的所有元素
echo ${array_name[@]}
```

#### 获取数组的长度
📃 例如：

```shell
#获取数组长度的方法与获取字符串长度的方法相同
#取得数组元素的个数
length=${#array_name[@]}
#或者
length=${#array_name[*]}
#取得数组单个元素的长度
lengthn=${#array_name[n]}
```


#### 关联数组

Bash 支持关联数组，可以使用任意的字符串、或者整数作为下标来访问数组元素。关联数组使用 declare 命令来声明。

🔦 语法格式：`declare -A array_name`

`-A `选项就是用于声明一个关联数组。

关联数组的键是唯一的。

📃 创建一个关联数组 site，并创建不同的键值

`declare -A site=(["A"]="1" ["B"]="2" ["C"]="3")`
📃 也可以先声明一个关联数组，然后再设置键和值

```
declare -A site
site["A"]="1"
site["B"]="2"
site["C"]="3"
```


📃 访问关联数组元素可以使用指定的键

```
declare -A site
site["A"]="1"
site["B"]="2"
site["C"]="3"
echo ${site["B"]}  # 执行脚本，输出结果：2
```


📃 在数组前加一个感叹号!可以获取数组的所有键

```shell
declare -A site
site["A"]="1"
site["B"]="2"
site["C"]="3"

echo "数组的键为: ${!site[*]}" # A B C
echo "数组的键为: ${!site[@]}" # A B C
```

📃 数组的值也可以写入变量

```shell
A=1
my_array=($A B C D)
echo "第一个元素为: ${my_array[0]}"  # 1
echo "第二个元素为: ${my_array[1]}"  # b
echo "第三个元素为: ${my_array[2]}"  # c
echo "第四个元素为: ${my_array[3]}"  # d
```

📃 字符串转数组

```shell
words="aaa bbb ccc"

#字符串转数组，空格是分隔符
array=(${words// / })  
#打印数组最后一个成员
echo ${array[${#array[*]}-1]}  # ccc
#打印数组长度
echo ${#array[*]}  # 3

#字符串不转换为数组，在循环实现以空格为分隔符打印每个成员
for word in ${words}; do
    echo ${word}       # aaabbbccc
done
```

📃 字符串替换

```shell
#使用 string/pattern/string 进行首个 pattern 的替换

string="text, dummy, text, dummy"
echo ${string/text/TEXT}   # TEXT, dummy, text, dummy

#使用 string//pattern/string 进行全部 pattern 的替换

string="text, dummy, text, dummy"
echo ${string//text/TEXT}  # TEXT, dummy, TEXT, dummy
```

## 运算符

![image-20240307212021271](./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240307212021271.png)

```shell
a=1
a=$(($a+1))   #不需要空格
a=$[$a+1]   #不需要空格
```

## read 命令
read 命令一个一个词组地接收输入的参数，每个词组需要使用空格进行分隔；如果输入的词组个数大于需要的参数个数，则多出的词组将被作为整体为最后一个参数接收。

测试文件 test.sh 代码如下：

```shell
#!/bin/bash
read firstStr secondStr
echo "第一个参数:$firstStr; 第二个参数:$secondStr"

# 执行测试：

$ sh test.sh 
一 二 三 四
第一个参数:一; 第二个参数:二 三 四

read -p "请输入一段文字:" -n 6 -t 5 -s password
echo -e "\npassword is $password"
参数说明：

-p 输入提示文字
-n 输入字符长度限制(达到6位，自动结束)
-t 输入限时
-s 隐藏输入内容

$ sh test.sh 
请输入一段文字:
password is abcdef
```

## 条件判断

![image-20240307213608420](./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240307213608420.png)

<img src="./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240307213859377.png" alt="image-20240307213859377" style="zoom:80%;" />

![image-20240307214002506](./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240307214002506.png)

🔗 条件表达式要放在方括号之间，并且要有空格。`[ $a==$b] `是错误的，必须写成 `[ $a == $b ]`。==注意 ==两边有空格，[]和变量有空格==

==true对应值为0，false对应值为1==

| ==   | 相等。用于比较两个数字，相同则返回 true |
| ---- | --------------------------------------- |
| =    | 检测两个字符串是否相等，相等返回 true   |

## IF控制

if 语法格式：

```shell
if condition
then
    command1 
    command2
    ...
    commandN 
fi

#或者
if [ 条件判断式 ] ;then
	command
fi
```

写成一行（适用于终端命令提示符）：

`a=10`

`if [ $a -gt 8 ]; then echo "true"; fi`

if else 语法格式

```shell
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


if else-if else 语法格式

```shell
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
📍 if else 的 […] 判断语句中大于使用 -gt，小于使用 -lt。
```

==📍 如果使用 ((…)) 作为判断语句，大于和小于可以直接使用 > 和 <。==

📃 参考实例

```shell
a=10
b=20
if [ $a == $b ]
then
   echo "a 等于 b"
elif [ $a -gt $b ]
then
   echo "a 大于 b"
elif [ $a -lt $b ]
then
   echo "a 小于 b"
else
   echo "没有符合的条件"
fi

# 或者 

a=10
b=20
if (( $a == $b ))
then
   echo "a 等于 b"
elif (( $a > $b ))
then
   echo "a 大于 b"
elif (( $a < $b ))
then
   echo "a 小于 b"
else
   echo "没有符合的条件"
fi
```

```shell
# 与 test 命令结合使用
num1=$[2*3]
num2=$[1+5]
if test $[num1] -eq $[num2]
then
    echo '两个数字相等!'
else
    echo '两个数字不相等!'
fi

```

## for 循环

### 格式1

![image-20240308123251917](./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240308123251917.png)

### for循环格式2

```shell
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
# 或者

for 变量 in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

📃 参考实例

```shell
for loop in 1 2 3 4 5
do
    echo "The value is: $loop"
done

# 输出结果：

The value is: 1
The value is: 2
The value is: 3
The value is: 4
The value is: 5


for str in This is a string
do
    echo $str
done

# 输出结果：
This
is
a
string

```



## while 语句

语法格式

```shell
while condition
do
    command
done
```

📃 参考实例

```shell
int =1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done
#输出结果
1
2
3
4
5
```

## until 循环
until 循环执行一系列命令直至条件为 true 时停止。

until 循环与 while 循环在处理方式上刚好相反。

一般 while 循环优于 until 循环，但在某些时候—也只是极少数情况下，until 循环更加有用。

until 语法格式

```shell
until condition  #condition 一般为条件表达式，如果返回值为 false，则继续执行循环体内的语句，否则跳出循环。
do
    command
done
```

📃 参考实例

```shell
#!/bin/bash                #使用sh或者dash会错误！！！！
a=0
until [ ! $a -lt 10 ]
do
   echo $a
   a=$[$a+1]
done
# 输出结果为：
0
1
2
3
4
5
6
7
8
9
10
```

## case 语句
case … esac 为多选择语句，与其他语言中的 switch … case 语句类似，是一种多分支选择结构，每个 case 分支用右圆括号开始，用两个分号 ;; 表示 break，即执行结束，跳出整个 case … esac 语句，esac（就是 case 反过来）作为结束标记。

语法格式

```shell
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


case 取值后面必须为单词 in，每一模式必须以右括号结束。取值可以为变量或常数，匹配发现取值符合某一模式后，其间所有命令开始执行直至 ;; 。

取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式。如果无一匹配模式，使用星号 * 捕获该值，再执行后面的命令。

📃 参考实例

```shell
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
1)  
	echo '你选择了 1'
;;
2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```

## break 和 continue
break 命令允许跳出所有循环（终止执行后面的所有循环）。

continue 命令与 break 命令类似，它不会跳出所有循环，仅仅跳出当前循环。

📃 参考实例

```shell
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的!"
            break
        ;;
    esac
done
```

```shell
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字: "
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的!"
            continue
            echo "结束"
        ;;
    esac
done
```

## let

<img src="./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240308124836122.png" alt="image-20240308124836122" style="zoom:80%;" />

## 函数

### basename
基本语法： `basename [string / pathname] [suffix]`

basename 命令会删掉所有的前缀包括最后一个（‘/’）字符，然后将字符串显示出来。

basename 可以理解为取路径里的文件名称.

suffix 为后缀，如果 suffix 被指定了，basename 会将 pathname 或 string 中的 suffix 去掉。

📃 参考实例

```shell
# 截取该/home/user1/banzhang.txt 路径的文件名称

[root@Demo shells]$ basename /home/user1/banzhang.txt
banzhang.txt
[root@Demo shells]$ basename /home/user1/banzhang.txt .txt
banzhang
```

### dirname
dirname 文件绝对路径

从给定的包含绝对路径的文件名中去除文件名，然后返回剩下的路径。

dirname 可以理解为取文件路径的绝对路径名称。

📃 参考实例

```shell
# 获取 banzhang.txt 文件的路径

[atguigu@hadoop101 ~]$ dirname /home/user1/banzhang.txt
/home/user1
```

### 自定义函数
定义格式

```shell
[ function ] funname [()]
{
action;
[return int;]
}
```

可以带function fun() 定义，也可以直接fun() 定义,不带任何参数。

参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return后跟数值n(0-255）。

```shell
demoFun(){
    echo "这是我的第一个 shell 函数!"
}
echo "-----函数开始执行-----"
demoFun
echo "-----函数执行完毕-----"

# 输出结果

-----函数开始执行-----
这是我的第一个 shell 函数!
-----函数执行完毕-----
```

```shell
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

# 输出类似下面：

这个函数会对输入的两个数字进行相加运算...
输入第一个数字: 
1
输入第二个数字: 
2
两个数字分别为 1 和 2 !
输入的两个数字之和为 3 !
```


在Shell中，调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数…

📃 参考实例

```shell
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

# 输出结果：

第一个参数为 1 !
第二个参数为 2 !
第十个参数为 10 !
第十个参数为 34 !
第十一个参数为 73 !
参数总数有 11 个!
作为一个字符串输出所有参数 1 2 3 4 5 6 7 8 9 34 73 !

# $10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数。
```

```
参数处理	说明
$#	传递到脚本或函数的参数个数
$*	以一个单字符串显示所有向脚本传递的参数
$$	脚本运行的当前进程ID号
$!	后台运行的最后一个进程的ID号
$@	与$*相同，但是使用时加引号，并在引号中返回每个参数。
$-	显示Shell使用的当前选项，与set命令功能相同。
$?	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。
```

## 输入/输出重定向

```
命令	说明
command > file	将输出重定向到 file。
command < file	将输入重定向到 file。
command >> file	将输出以追加的方式重定向到 file。
n > file	将文件描述符为 n 的文件重定向到 file。
n >> file	将文件描述符为 n 的文件以追加的方式重定向到 file。
n >& m	将输出文件 m 和 n 合并。
n <& m	将输入文件 m 和 n 合并。
<< tag	将开始标记 tag 和结束标记 tag 之间的内容作为输入。
```


文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。

### 输出重定向

语法：`command1 > file1`

上面这个命令执行command1然后将输出的内容存入file1。

任何file1内的已经存在的内容将被新内容替代。如果要将新内容添加在文件末尾，请使用>>操作符。

📃 参考实例

```shell
$ echo "AAAAA" > users
$ cat users
AAAAA
$

# 如果不希望文件内容被覆盖，可以使用 >> 追加到文件末尾，例如：

$ echo "AAAAA" >> users
$ cat users
AAAAA
AAAAA
$
```

### 输入重定向

语法：` command1 < file1`

📃 参考实例

```shell
# 接着以上实例，我们需要统计 users 文件的行数,执行以下命令：

$ wc -l users
      2 users
       
#也可以将输入重定向到 users 文件：
$  wc -l < users
       2        
# 上面两个例子的结果不同：第一个例子，会输出文件名；第二个不会，因为它仅仅知道从标准输入读取内容。
# 同时替换输入和输出，执行command1，从文件infile读取内容，然后将输出写入到outfile中。
command1 < infile > outfile 
```

一般情况下，每个 Unix/Linux 命令运行时都会打开三个文件：

    👉 标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
    
    👉 标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
    
    👉 标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。

## EOF

<img src="./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240308131819428.png" alt="image-20240308131819428" style="zoom:80%;" />

将EOF临时文件写入user

## 简单正则使用
正则表达式使用单个字符串来描述、匹配一系列符合某个语法规则的字符串。在很多文本编辑器里，正则表达式通常被用来检索、替换那些符合某个模式的文本。在 Linux 中，grep，sed，awk 等文本处理工具都支持通过正则表达式进行模式匹配。

### 常用的特殊字符

```shell
#  ^ 匹配一行的开头
[root@Demo shells]$ cat /etc/passwd | grep ^a 
# 会匹配出所有以 a 开头的行

# $ 匹配一行的结束
[root@Demo shells]$ cat /etc/passwd | grep t$
# 会匹配出所有以 t 结尾的行

#  . 匹配一个任意的字符
[root@Demo shells]$ cat /etc/passwd | grep r..t
# 会匹配包含 rabt,rbbt,rxdt,root 等的所有行


#  * 不单独使用，他和上一个字符连用，表示匹配上一个字符 0 次或多次
[root@Demo shells]$ cat /etc/passwd | grep ro*t
# 会匹配 rt, rot, root, rooot, roooot 等所有行
```

### 字符区间（中括号）：[ ]

```shell
[ ] 表示匹配某个范围内的一个字符
[6,8] ------ 匹配 6 或者 8
[0-9] ------ 匹配一个 0-9 的数字
[0-9]* ------ 匹配任意长度的数字字符串
[a-z] ------匹配一个 a-z 之间的字符
[a-z]* ------ 匹配任意长度的字母字符串
[a-c, e-f] ----- 匹配 a-c 或者 e-f 之间的任意字符

```

### 转义符 \

```shell

# \ 表示转义，并不会单独使用。由于所有特殊字符都有其特定匹配模式，当我们想匹配某一特殊字符本身时（例如，我想找出所有包含 ‘$’ 的行），就会碰到困难。此时我们就要将转义字符和特殊字符连用，来表示特殊字符本身。
[root@Demo shells]$ cat /etc/passwd | grep ‘a\$b’ 
# 就会匹配所有包含 a$b 的行。注意需要使用单引号将表达式引起来
```

## 文本处理

### cut 命令
cut 的工作就是“剪”，具体的说就是在文件中负责剪切数据用的。==cut 命令从文件的每一行==剪切字节、字符和字段并将这些字节、字符和字段输出。

🌊 语法： `cut [参数] [file]`

cut 命令从文件的每一行剪切字节、字符和字段并将这些字节、字符和字段写至标准输出。

如果不指定 File 参数，cut 命令将读取标准输入。必须指定 -b、-c 或 -f 标志之一。

```
参数	说明
-b	以字节为单位进行分割。这些字节位置将忽略多字节字符边界，除非也指定了 -n 标志。
-c	以字符为单位进行分割。
-d	自定义分隔符，默认为制表符。 delimiter
-f	-d一起使用，指定显示哪个区域。
-n	取消分割多字节字符。仅和 -b 标志一起使用。如果字符的最后一个字节落在由 -b 标志的 List 参数指示的范围之内，该字符将被写出；否则，该字符将被排除
```

📃 参考实例

```shell
# 当执行who命令时，会输出类似如下的内容：

$ who
rocrocket :0           2009-01-08 11:07
rocrocket pts/0        2009-01-08 11:23 (:0.0)
rocrocket pts/1        2009-01-08 14:15 (:0.0)

# 如果我们想提取每一行的第3个字节，如下：

$ who|cut -b 3
c
c
```

```shell
# 数据准备
$ vim cut.txt 
```

<img src="./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240308134825182.png" alt="image-20240308134825182" style="zoom:80%;" />

```shell
$ cut -d " " -f 1 cut.txt # 以 " "分割 截取第一列
$ cut -d " " -f 2,3 cut.txt # 以 " "分割 截取第2,3列
```

```shell
$ cat /etc/passwd |grep bash$ #搜索bash结尾的用户
$ cat /etc/passwd |grep bash$| cut -d ":" -f 1,6,7  #截取1，6，7列
```

<img src="./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240308135505141.png" alt="image-20240308135505141" style="zoom:80%;" />

```shell
$ cat /etc/passwd |grep bash$| cut -d ":" -f 1-4,6-  #截取1-4列，6到最后列
```

```shell
$ ifconfig ens33|grep network|cut -d " " f 10  
#grep搜到network这一行 cut切第10列
```

<img src="./image/image_12,shell%E7%BC%96%E7%A8%8B/image-20240308140139864.png" alt="image-20240308140139864" style="zoom:80%;" />

### 文件包含

Shell 也可以包含外部脚本。这样可以很方便的封装一些公用的代码作为一个独立的文件。

语法格式：

```shell
. filename   # 注意点号(.)和文件名中间有一空格
# 或
source filename
```


📃 参考实例

创建两个 shell 脚本文件。

1️⃣ test1.sh

```
#!/bin/bash
url="http://www.baidu.com"
```

2️⃣ test2.sh

```
#!/bin/bash
. ./test1.sh  #使用 . 号来引用test1.sh 文件，或者 source ./test1.sh
echo "百度地址：$url"
```

3️⃣ 为 test2.sh 添加可执行权限并执行

```shell
$ chmod +x test2.sh 
$ ./test2.sh 
#运行结果： 百度地址：http://www.runoob.com
# 被包含的文件 test1.sh 不需要可执行权限。
```

### sed 命令 stream editor

==sed 命令是利用脚本来处理文本文件==。

sed 可依照脚本的指令来处理、编辑文本文件。

sed 主要用来自动编辑一个或多个文件、简化对文件的反复操作、编写转换程序等。

🌊 **语法：** `sed [-hnV][-e<script>][-f<script文件>][文本文件]`
     ```
     参数	说明
     -e<script>	以选项中指定的script来处理输入的文本文件
     -f<script文件>	以选项中指定的script文件来处理输入的文本文件
     -h	显示帮助
     -n	仅显示script处理后的结果
     -V	显示版本信息
     ```

#### 参数说明

==命令全部用‘ ’ 引起来==

```
动作	说明
a	新增， a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)
c	取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！
d	删除，因为是删除，所以 d 后面通常不接任何东西；
i	插入， i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)
p	打印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行
s	取代，可以直接进行取代的工作！通常这个 s 的动作可以搭配正则表达式
```

📃 参考实例

0️⃣ 创建一个 testfile 文件

```shell
$ cat testfile #查看testfile 中的内容  
HELLO LINUX!  
Linux is a free unix-type opterating system.  
This is a linux testfile!  
Linux test 
Google
Taobao
Runoob
Tesetfile
Wiki
```

#### 以行为单位的新增/删除

==每一行之间都必须要以反斜杠 \ 来进行标记==

1️⃣ 在 testfile 文件的第四行后添加一行，并将结果输出到标准输出

```shell
sed -e 4a\newLine testfile 
```

2️⃣ 将 testfile 的内容列出并且列印行号，同时，将第 2~5 行删除

```shell
$ nl testfile | sed '2,5d'
     1  HELLO LINUX!  
     6  Taobao
     7  Runoob
     8  Tesetfile
     9  Wiki
```

3️⃣ 只要删除第 2 行

```shell
$ nl testfile | sed '2d'
     1  HELLO LINUX!  
     3  This is a linux testfile!  
     4  Linux test 
     5  Google
     6  Taobao
     7  Runoob
     8  Tesetfile
     9  Wiki
```

4️⃣ 要删除第 3 到最后一行

```shell
$ nl testfile | sed '3,$d' 
     1  HELLO LINUX!  
     2  Linux is a free unix-type opterating system.  

```

5️⃣ 在第二行后(即加在第三行) 加上drink tea? 字样

```shell
$ nl testfile | sed '2a drink tea'
     1  HELLO LINUX!  
     2  Linux is a free unix-type opterating system.  
drink tea
     3  This is a linux testfile!  
     4  Linux test 
     5  Google
     6  Taobao
     7  Runoob
     8  Tesetfile
     9  Wiki
```

6️⃣ 如果是要在第二行前，命令如下

```shell
$ nl testfile | sed '2i drink tea' 
     1  HELLO LINUX!  
drink tea
     2  Linux is a free unix-type opterating system.  
     3  This is a linux testfile!  
     4  Linux test 
     5  Google
     6  Taobao
     7  Runoob
     8  Tesetfile
     9  Wiki
```

7️⃣如果是要增加两行以上，在第二行后面加入两行字，例如 Drink tea or … 与 drink beer?

```shell
$ nl testfile | sed '2a Drink tea or ......\
drink beer ?'

     1  HELLO LINUX!  
     2  Linux is a free unix-type opterating system.  

Drink tea or ......
drink beer ?
     3  This is a linux testfile!  
     4  Linux test 
     5  Google
     6  Taobao
     7  Runoob
     8  Tesetfile
     9  Wiki
```

#### 以行为单位的替换与显示

1️⃣ 将第 2-5 行的内容取代成为 No 2-5 number

```shell
$ nl testfile | sed '2,5c No 2-5 number'
     1  HELLO LINUX!  
No 2-5 number
     6  Taobao
     7  Runoob
     8  Tesetfile
     9  Wiki
```

2️⃣仅列出 testfile 文件内的第 5-7 行

```
$ nl testfile | sed -n '5,7p'
     5  Google
     6  Taobao
     7  Runoob
```

#### 数据的搜寻并显示

1️⃣ 搜索 testfile 有 oo 关键字的行

```shell
$ nl testfile | sed -n '/oo/p'
     5  Google
     7  Runoob
```


如果 root 找到，除了输出所有行，还会输出匹配行。

#### 数据的搜寻并删除

1️⃣ 删除 testfile 所有包含 oo 的行，其他行输出

```shell
$ nl testfile | sed  '/oo/d'
     1  HELLO LINUX!  
     2  Linux is a free unix-type opterating system.  
     3  This is a linux testfile!  
     4  Linux test 
     6  Taobao
     8  Tesetfile
     9  Wiki
```

#### 数据的搜寻并执行命令

1️⃣ 搜索 testfile，找到 oo 对应的行，执行后面花括号中的一组命令，每个命令之间用分号分隔，这里把 oo 替换为 kk，再输出这行

```
$ nl testfile | sed -n '/oo/{s/oo/kk/;p;q}'  # 最后的 q 是退出
     5  Gkkgle
```

#### 数据的查找与替换

```
# sed 的查找与替换的与 vi 命令类似，语法格式如下

sed 's/要被取代的字串/新的字串/g'
```

1️⃣ 将 testfile 文件中每行第一次出现的 oo 用字符串 kk 替换

`sed -e 's/oo/kk/' testfile`

2️⃣ g 标识符表示全局查找替换，使 sed 对文件中所有符合的字符串都被替换，修改后内容会到标准输出，不会修改原文件

`sed -e 's/oo/kk/g' testfile`
3️⃣ 选项 i 使 sed 修改文件

`sed -i 's/oo/kk/g' testfile`
4️⃣ 批量操作当前目录下以 test 开头的文件

`sed -i 's/oo/kk/g' ./test*`
5️⃣ 使用 /sbin/ifconfig 查询 IP，将 IP 前，后的部分予以删除

```shell
$ /sbin/ifconfig eth0
eth0 Link encap:Ethernet HWaddr 00:90:CC:A6:34:84
inet addr:192.168.1.100 Bcast:192.168.1.255 Mask:255.255.255.0
inet6 addr: fe80::290:ccff:fea6:3484/64 Scope:Link
UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1
.....(以下省略).....

# 本机的 ip 是 192.168.1.100

# 将 IP 前面的部分予以删除

$ /sbin/ifconfig eth0 | grep 'inet addr' | sed 's/^.*addr://g'

# 192.168.1.100 Bcast:192.168.1.255 Mask:255.255.255.0

# 接下来则是删除后续的部分，即：192.168.1.100 Bcast:192.168.1.255 Mask:255.255.255.0

# 将 IP 后面的部分予以删除

$ /sbin/ifconfig eth0 | grep 'inet addr' | sed 's/^.*addr://g' | sed 's/Bcast.*$//g'
192.168.1.100
```

#### 多点编辑

1️⃣ 一条 sed 命令，删除 testfile 第三行到末尾的数据，并把 HELLO 替换为 RUNOOB :

```
$ nl testfile | sed -e '3,$d' -e 's/HELLO/RUNOOB/'
     1  RUNOOB LINUX!  
     2  Linux is a free unix-type opterating system.  
```

-e 表示多点编辑，第一个编辑命令删除 testfile 第三行到末尾的数据，第二条命令搜索 HELLO 替换为 RUNOOB。

#### 直接修改文件内容

1️⃣ regular_express.txt 文件内容

```
$ cat regular_express.txt 
runoob.
google.
taobao.
facebook.
zhihu-
weibo-
```

2️⃣ 利用 sed 将 regular_express.txt 内每一行结尾若为 . 则换成 !

```
$ sed -i 's/\.$/\!/g' regular_express.txt
$ cat regular_express.txt 
runoob!
google!
taobao!
facebook!
zhihu-
weibo-:q:q 
```

3️⃣利用 sed 直接在 regular_express.txt 最后一行加入 # This is a test

```
$ sed -i '$a # This is a test' regular_express.txt
$ cat regular_express.txt 
runoob!
google!
taobao!
facebook!
zhihu-
weibo-
# This is a test
```

$ 代表的是最后一行，而 a 的动作是新增，因此该文件最后新增 # This is a test！

## awk命令
AWK 是一种处理文本文件的语言，是一个强大的文本分析工具，把文件逐行的读入，以空格为默认分隔符将每行切片，切开的部分再进行分析处理。

==AWK 是取了三位创始人 ==Alfred Aho，Peter Weinberger, 和 Brian Kernighan 的 Family Name 的首字符。

🌊 语法：` awk [选项参数] ‘/pattern1/{action1} /pattern2/{action2}...’ filename`

pattern：表示 awk 在数据中查找的内容，就是匹配模式
action：在找到匹配内容时所执行的一系列命令

```
参数	说明
-F	指定输入文件分隔符
-v	赋值一个用户定义变量
```

#### 基本使用

0️⃣ log.txt文本内容

```shell
2 this is a test
3 Do you like awk
This's a test
10 There are orange,apple,mongo
```


1️⃣ 用法一： `awk '{[pattern] action}' {filenames} `行匹配语句 awk ‘’ 只能用单引号

每行按空格或TAB分割，输出文本中的1、4列

```shell
 $ awk '{print $1,$4}' log.txt
 2 a
 3 like
 This's
 10 orange,apple,mongo
```

格式化输出

 ```
 $ awk '{printf "%-8s %-10s\n",$1,$4}' log.txt
  2        a
  3        like
  This's
  10       orange,apple,mongo
 ```

2️⃣ 用法二： awk -F -F相当于内置变量FS, 指定分割字符

使用","分割

```
 $  awk -F, '{print $1,$2}'   log.txt
 2 this is a test
 3 Do you like awk
 This's a test
 10 There are orange apple
```

使用内建变量分隔

```
 $ awk 'BEGIN{FS=","} {print $1,$2}'     log.txt
 2 this is a test
 3 Do you like awk
 This's a test
 10 There are orange apple
```

使用多个分隔符.先使用空格分割，然后对分割结果再使用","分割

```
 $ awk -F '[ ,]'  '{print $1,$2,$5}'   log.txt
 2 this test
 3 Do awk
 This's a
 10 There apple
```



3️⃣ 用法三：`awk -v`设置变量

```
 $ awk -va=1 '{print $1,$1+a}' log.txt
 2 3
 3 4
 This's 1
 10 11
```

```
 $ awk -va=1 -vb=s '{print $1,$1+a,$1b}' log.txt
 2 3 2s
 3 4 3s
 This's 1 This'ss
 10 11 10s
```

4️⃣ 用法四： awk -f {awk脚本} {文件名}

` $ awk -f cal.awk log.txt`
#### 运算符

```
运算符	描述
= += -= *= /= %= ^= **=	赋值
?:	C条件表达式
||	逻辑或
&&	逻辑与
~ 和 !~	匹配正则表达式和不匹配正则表达式
< <= > >= != ==	关系运算符
空格	连接

+ -	加，减

* / %	乘，除与求余

+ - !	一元加，减和逻辑非
    ^ ***	求幂
    ++ –	增加或减少，作为前缀或后缀
    $	字段引用
    in	数组成员
```

📃 参考实例

过滤第一列大于2的行

```
$ awk '$1>2' log.txt

# 输出

3 Do you like awk
This's a test
10 There are orange,apple,mongo
```

过滤第一列等于2的行

```
$ awk '$1==2 {print $1,$3}' log.txt

# 输出

2 is
```

过滤第一列大于2并且第二列等于’Do’的行

```
$ awk '$1>2 && $2=="Do" {print $1,$2,$3}' log.txt 

# 输出

3 Do you
```

#### 内建变量

```
变量	描述
$n	当前记录的第n个字段，字段间由FS分隔
$0	完整的输入记录
ARGC	命令行参数的数目
ARGIND	命令行中当前文件的位置(从0开始算)
ARGV	包含命令行参数的数组
CONVFMT	数字转换格式(默认值为%.6g)ENVIRON环境变量关联数组
ERRNO	最后一个系统错误的描述
FIELDWIDTHS	字段宽度列表(用空格键分隔)
FILENAME	当前文件名
FNR	各文件分别计数的行号
FS	字段分隔符(默认是任何空格)
IGNORECASE	如果为真，则进行忽略大小写的匹配
NF	一条记录的字段的数目
NR	已经读出的记录数，就是行号，从1开始
OFMT	数字的输出格式(默认值是%.6g)
OFS	输出字段分隔符，默认值与输入字段分隔符一致。
ORS	输出记录分隔符(默认值是一个换行符)
RLENGTH	由match函数所匹配的字符串的长度
RS	记录分隔符(默认是一个换行符)
RSTART	由match函数所匹配的字符串的第一个位置
SUBSEP	数组下标分隔符(默认值是/034)
```

📃 参考实例

```
$ awk 'BEGIN{printf "%4s %4s %4s %4s %4s %4s %4s %4s %4s\n","FILENAME","ARGC","FNR","FS","NF","NR","OFS","ORS","RS";printf "---------------------------------------------\n"} {printf "%4s %4s %4s %4s %4s %4s %4s %4s %4s\n",FILENAME,ARGC,FNR,FS,NF,NR,OFS,ORS,RS}'  log.txt

FILENAME ARGC  FNR   FS   NF   NR  OFS  ORS   RS
---------------------------------------------

log.txt    2    1         5    1
log.txt    2    2         5    2
log.txt    2    3         3    3
log.txt    2    4         4    4
```

```
$ awk -F\' 'BEGIN{printf "%4s %4s %4s %4s %4s %4s %4s %4s %4s\n","FILENAME","ARGC","FNR","FS","NF","NR","OFS","ORS","RS";printf "---------------------------------------------\n"} {printf "%4s %4s %4s %4s %4s %4s %4s %4s %4s\n",FILENAME,ARGC,FNR,FS,NF,NR,OFS,ORS,RS}'  log.txt

FILENAME ARGC  FNR   FS   NF   NR  OFS  ORS   RS
---------------------------------------------

log.txt    2    1    '    1    1
log.txt    2    2    '    1    2
log.txt    2    3    '    2    3
log.txt    2    4    '    1    4
```

输出顺序号 NR, 匹配文本行号

```
$ awk '{print NR,FNR,$1,$2,$3}' log.txt
1 1 2 this is
2 2 3 Do you
3 3 This's a test
4 4 10 There are
```

指定输出分割符

```
$  awk '{print $1,$2,$5}' OFS=" $ "  log.txt
2 $ this $ test
3 $ Do $ awk
This's $ a $
10 $ There $
```

使用正则，字符串匹配，输出第二列包含 “th”，并打印第二列与第四列

```
$ awk '$2 ~ /th/ {print $2,$4}' log.txt     # ~ 表示模式开始。// 中是模式。
this a

# 输出包含 "re" 的行

$ awk '/re/ ' log.txt
10 There are orange,apple,mongo
```

忽略大小写

```
$ awk 'BEGIN{IGNORECASE=1} /this/' log.txt
2 this is a test
This's a test
```

模式取反

```
$ awk '$2 !~ /th/ {print $2,$4}' log.txt
Do like
a
There orange,apple,mongo

$ awk '!/th/ {print $2,$4}' log.txt
Do like
a
There orange,apple,mongo
```


#### awk脚本

BEGIN{ 这里面放的是执行前的语句 }

END {这里面放的是处理完所有的行后要执行的语句 }

📃 参考实例

有这么一个文件（成绩表）：

```
$ cat score.txt
Marry   2143 78 84 77
Jack    2321 66 78 45
Tom     2122 48 77 71
Mike    2537 87 97 95
Bob     2415 40 57 62
```

awk 脚本如下

```
$ cat cal.awk
#!/bin/awk -f
#运行前
BEGIN {
    math = 0
    english = 0
    computer = 0

    printf "NAME    NO.   MATH  ENGLISH  COMPUTER   TOTAL\n"
    printf "---------------------------------------------\n"

}
#运行中
{
    math+=$3
    english+=$4
    computer+=$5
    printf "%-6s %-6s %4d %8d %8d %8d\n", $1, $2, $3,$4,$5, $3+$4+$5
}
#运行后
END {
    printf "---------------------------------------------\n"
    printf "  TOTAL:%10d %8d %8d \n", math, english, computer
    printf "AVERAGE:%10.2f %8.2f %8.2f\n", math/NR, english/NR, computer/NR
}
```

执行结果

```
$ awk -f cal.awk score.txt

NAME    NO.   MATH  ENGLISH  COMPUTER   TOTAL
---------------------------------------------

Marry  2143     78       84       77      239
Jack   2321     66       78       45      189
Tom    2122     48       77       71      196
Mike   2537     87       97       95      279

Bob    2415     40       57       62      159
---------------------------------------------

  TOTAL:       319      393      350
AVERAGE:     63.80    78.60    70.00
```



