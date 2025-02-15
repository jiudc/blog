---
title: Shell脚本
categories:
  - 工作
  - Linux
tags:
  - shell
  - 脚本
date: 2025-02-15 21:12:20
---
变量

### 系统变量

- HOME
- PATH

### 申明变量

- 变量=
- 撤销：unset 变量
- 申明静态变量：readonly 变量，不能unset
- 提升为全局变量，可供其他shell使用：export 变量名

### 特殊变量

- $0：代表该脚本名称
- $1~$9，${10}
- $#：获取参数个数
- $*：代表命令行中所有的参数，会把所有参数看错一个整体
- $@：代表命令行中的所有参数，把每个参数区分对待
- $?：上一个命令的返回值

## 运算符

### 基本语法

- ${运算式}或$(())
- expr +,-,\\*,/,% ==expr运算符间要有空格==
    - expr 3 + 2
    - expr `expr 2 + 3` \* 4
    - $((2+3)*4)

## 条件判断

### 基本语法

- [ condition ] ==condition前后要有空格==

1. 两个整数之间
1. = 字符串比较
2. -lt
3. -le
4. -eq
5. -gt
6. -ge
7. -ne
2. 按照文件权限判断
1. -r
2. -w
3. -x
3. 按照文件类型进行判断
1. -f 文件存在并且是一个常规的文件
2. -d 文件存在并且是一个目录
3. -e 文件存在

eg：

- [ 23 -ge 22 ]

- [ -w hello.sh ]


4. 多条件判断
1. [ condition ] && [ condition ]
2. [ condition ] || [condition]

## 流程控制

### if判断

```shell
#!/bin/bash
if [ 条件判断式 ];then
    程序
fi
if [ 条件判断式 ]
    then
        程序
fi
```

### case 语句

```shell
case $变量名 in
"值1")
    程序
    ;;;
"值2")
    程序
    ;;;
*)
    ;;;
esac
```

### for 循环

```shell
#!/bin/bash
s=0
for ((i=1;i<100;i++))
do
    s=$[$s+$i]
done
echo $s
```

```shell
#!/bin/bash
for i in $*
do
    echo "$i"
done
```

“$*”一个整体，“$@‘’、$@、$*一样

### while

```shell
#!/bin/bash
i=1
s=0
while [ $i -le 100 ]
do
    s=$[$s + $i]
    i=$[$i + 1]
done
```

## read读取控制台输入

- read
    - -p:指定读取时的提示符
    - -t:指定读取时的等待时间
        - read -t 7 -p “Enter your name in 7 seconds” NAME

## 执行控制

- set
    - -e：若指令传回值不等于零，则立即退出
    - -x：执行指令后，会先显示该指令及所下的参数
    - -o pipefail：设置这个选项，包含管道命令的语句的返回值，会变成最后一个返回非零的管道命令的返回值

## 引号

- 反引号：命令的替换
- 双引号：弱引用，可实现变量和命令替换，同$()
- 单引号：强引用，不完成变量替换

## 函数

### 系统函数

- basename [string/pathname] [suffix]
    - 将pathname或string中的suffix去除，获取文件名称
- dirname 文件绝对路径
    - 返回路径

### 自定义函数

```shell
[ function ] funname [())]
{
    Action;
    [return init;]
}
```

- 函数必须先声明，后使用
- 返回值，只能通过$?(0-255)

## Shell工具

### cut

- cut [选项参数] filename：从每一行中剪切字符

    - -f：列号，提取第激烈

    - -d：分隔符，按照分隔符分隔列

      ```shell
      cut -d " " -f 1,2 cut.txt
      cat cut.txt|grep guan|cut -d " " -f 1
      echo $PATH|cut -d : -f 3-
      ifconfig eth0|grep "inet addr"|cut -d : -f 2|cut -d " " -f 1
      ```


### sed

sed是一种流处理器，一次处理一行内容。处理时，把当前处理的行存储在临时缓冲区，模式空间，接着用sed命令处理缓冲区内容，处理完成后，把缓冲区内容送往屏幕。

- sed [选项参数] ’command‘ filename

    - -e：直接在指令列模式上进行sed的动作编辑

    - 命令功能

        - a：新增，在下一行出现

        - d：删除

        - s：查找并替换

          ```shell
          sed "2a xxx" sed.txt
          sed "/wo/d" sed.txt
          sed "s/wo/ni/g" sed.txt
          sed -e "2d" -e "s/wo/ni/g" sed.txt
          ```


    ### awk
    
    ​ 把文件逐行读入，一空格为默认分隔符将每行切开，切开的部分再进行分析处理。

- awk [选项参数] ’pattern1{action1} pattern2{action2}…’ filename

- -F:指定输入文件拆分隔符

- -v：赋值一个用户定义变量

  ```shell
  awk -F : '/^root/ {print $7}' passwd
  awk -F : '/^root/ {print $1","$7}' passwd
  awk -F : 'BEGIN{print "user,shell"} {print $1","$7 END{print "end"}' passwd
  awk -F : -v i=1 'print $3+i' passwd
  ```

- 内置变量

    - FILENAME：文件名
    - NR：已读的记录数
    - NF：浏览记录的域的个数（切割后，列的个数）

### sort

- sort

    - -n：按照数值大小

    - -r：倒序

    - -t：设置排序时所用的风格字符

    - -k：指定需要排序的列

      ```shell
      sort -t : -nrk 2
      ```


## 示例

- SPARK_HOME="$(cd "`dirname "$0"`/.."; pwd)"

## 输出重定向

# 脚本调试技术

## trap

用于捕获指定的信号并执行预定义的命令

trap ‘command’ signal

signal是要捕获的信号，command是捕获到信号之后，所要执行的命令。可使用kill -l命令看到系统中全部可用的信号名，捕获信号后所执行的命令可以是任何一条或多条合法的shell语句，也可以是一个函数名

shell脚本在执行时，会产生三个伪信号，因为这个三个信号是shell产生的，而其他的信号是操作系统产生的

| 信号名 | 何时产生 |
| --- | --- |
| EXIT | 从一个函数中退出或整个脚本执行完毕 |
| ERR | 当一条命令返回非零状态时（代表命令执行不成功） |
| DEBUG | 脚本中每一条命令执行前 |

$LINENO为shell内置变量，代表shell脚本的当前行号

DEBUG可用来对相关变量进行全程跟踪。

## tee

在shell脚本中管道以及输入输出重定向使用非常多，因为使用管道，中间结果不会打印在屏幕上，给调试带来的困难，可借助tee。

ipadd=`/sbin/ifconfig |grep 'inet addr:' |grep -v '127.0.0.1'|tee temp.txt|cut -d : -f3| awk '{print $1}'

## 使用调试钩子

在C语言中，常用DEBUG宏来控制是否输出调试信息，shell中可使用这样的机制。

```shell
if [[ "$DEBUG" == "true" ]];then
  echo "debugging"
fi
```

在脚本调试阶段，先执行export DEBUG=true命令来打开钩子

如果在每一处需要输出调试信息的地方用if语句来判断DEBUG变量的值，还是比较繁琐，通过定义一个DEBUG函数可更加简洁

```
DEBUG(){
  if [[ "$DEBUG" == "true" ]];then 
       $@
  fi
}
```

## 使用shell的执行选项

-n只读取shell脚本，但不实际执行
-x进入跟踪方式，显示执行的每一条命令
-c ’strings‘从strings中读取命令

-x模式：前面有+号的是shell脚本实际执行的命令，前面有++号的行是执行trap机制中指定的命令，其他的行则是输出信息。

```shell
set -x #开启-x
set +x #关闭-x
DEBUG set -x
DEBUG set +x
```

## 对“-x”选型的增强

shell内置的环境变量

$LINENO：代表shell脚本的当前行号，类似于C语言内置宏\_\_LINE\_\_ $FUNCNAME：函数的名字，是个数组变量，包含整个调用链上所有函数的名字，${FUNCNAME[0]}表示shell脚本当前正在执行的函数的名字，而${FUNCNAME[1]}则表示调用函数${FUNCNAME[0]}的函数的名字 $PS4:主提示符变量$PS1和第二级提示符变量$PS2比较常见，而$PS4为第四级提示符变量。$PS4的值将被显示在“-x”选项输出的每一条命令的前面。在Bash Shell，缺省的$PS4是“+”。可以使用内置变量重定义来增强"-x"

```shell
export PS4='{$LINENO:${FUNCNAME[0]}}'
```