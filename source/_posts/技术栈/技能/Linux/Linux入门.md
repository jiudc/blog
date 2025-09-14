---
title: Linux入门
categories:
  - 技术栈
  - 技能
  - Linux
tags:
  - Linux
date: 2025-02-15 21:10:36
---
# Linux

## Linux入门

​ 开源、免费的操作系统。

## Linux目录结构

- /bin：二进制目录，存放许多用户级的GNU工具00
- /boot：启动目录
- /dev：设备目录，Linux在这里创建设备节点
- /etc：系统配置文件目录
- /home：主目录，Linux在这里创建设备节点
- /lib：库目
- /media：媒体目录，可移动媒体设备常用的挂载点
- /mnt：挂载目录
- /opt：可选目录，用于存放第三方软件包和数据文件
- /proc：进程目录，存放现有硬件及当前进程
- /root：root用户的主目录
- /sbin：系统二进制目录
- /run：运行目录，存放系统运行时的运行时数据
- /srv：服务目录，存放本地服务相关文件
- /sys：系统目录，存放系统硬件信息的相关文件
- /tmp：临时文件
- /usr：用户二进制文件
- /var：可变目录，可存放经常变化的文件，如日志

## 基础命令

### 关机重启

- shutdown
    - shutdown -h now：立即关机
    - shutdown -h 1:一分钟后关机
    - shutdow -r now：立即重启
- halt：直接使用，等于关机
- reboot：重启系统
- syn：把内存的数据同步到磁盘
- logout：注销

### 用户管理

- 添加用户：useradd [选项] 用户（创建默认组；创建home目录）
    - useradd -d /home/trigger/ gemsuser
- 指定或修改密码：passwd gemsuser
- 删除用户：userdel 用户名
    - 保留home目录：userdel 用户
    - 不保留home目录：userdel -r 用户
- 查询用户信息：id 用户名
- 切换用户：su -
- 查询用户：whoami

### 用户组

- 组件组：groupadd 组名
- 删除组：groupdel 组名
- 增加用户指定组：useradd - g 用户组 用户名
- 修改用户组：usermod -g 用户组 用户名

### 用户和组的相关文件

- /etc/passwd :用户配置文件（用户信息）
- /etc/group：组配置文件（组信息），组名 口令 组ID 组内用户列表
- /etc/shadow：口令配置文件，密码等

## 实用指令

### 指定运行界别

- 0：关机
- 1：单用户（找回丢失密码）
- 2：多用户（无网络服务）
- 3：多用户（有网络服务）
- 4：保留
- 5：图形界面
- 6：重启

​ 系统的运行级别配置文件/etc/inittab

切换到运行级别的指令：init [0123456]

### 如何找回root密码

- 进入单用户模式，修改密码
- 开机->引导时输入回车键->e->选择第二行（编辑内核）再输入e->在这行最后输入1->回车键->b，进入单用户模式

### 帮助指令

man [命令或者配置文件] 指令

help 命令

### 文件目录类

- pwd
- ls：
    - -F,区分文件夹文件,-R递归,-a全部，-l显示长列表,ls -alF
    - -l my_script，过滤匹配输出列表
    - -l –time=atime，才可以显示文件的访问时间，否则默认显示修改时间
    - -d只列出目录
    - -i查看文件或者目录的inode编号
- mkdir -p,递归目录创建
- rmdir：删除空目录
- 创建文件：touch filename,也可用于改变修改日期，若想改变访问时间，可加上参数-a
- 复制：cp src dst，cp -i覆盖前会提示
- 递归复制：cp -r
    - \cp 强制覆盖，不提示覆盖
- rm
    - -r：递归真个文件夹
    - -f：不提示
- mv
- cat file，查看文件，-n行号，-b行数（不包括空行
    - cat -n /etc/profile |more
- cat -T不让制表符出现，一旦开始无法控制后面操作
- more显示文本文件，会在显示每页数据后停下来
    - 空格键：向下翻一页
    - Enter：一行
    - q：退出
    - Ctrl+F：向下滚动一屏
    - Ctrl+B：向上
    - =：输出当前的行号
    - ：f 输出文件名和当前的行号
    - ！ 调用shell，并执行命令
- less，more的升级版（查看大文件）
    - /pattern：匹配，n下一个，N前一个
    - ？pattern
    - CTRL+f\b\d\u
- 查看部分文件tail,默认情况显示文件的末尾10行
- tail -n 2 log_file显示末尾2行，-f参数允许其他进程使用该文件时查看文件类容，hi保持活动状态，并不断显示添加到文件的内容。实时监测系统日志
- head，显示开头，head -5 log_file
- 重定向和追加
    - \> 覆盖
    - \>\>追加
- 链接文件：ln -s src dst，软链接
    - 删除软连接时==不要带斜杠==
- ln src dst，硬链接，完全一样
- history
    - history 10
    - !178

### 时间日期类

- date
    - date
    - date “+%Y %m %d %H %M %S”
    - 设置日期：data -s “2020-2-2 22:22:22”
- cal：显示日历时间
    - cal
    - cal 2020

### 搜索查找类

- find
    - find / -name xxname
    - find / -user gemsuser
    - find / -size +20M/-10K/30G
- locate：在第一次运行前，必须使用updatedb指令创建locate数据库，可用于快速定位
    - updatedb
    - locate xxxx
- grep
    - -n：显示匹配行及行号
    - -i：忽略大小写

### 压缩和解压缩

- gzip：压缩后，不保留原文件。gzip xx

- gunzip

- zip：zip [选项] xxx.zip 目录

    - -r：递归压缩
- unzip

    - -d：解压目的目录
- tar：tzr -zvbf xx.tar.gz file1 file2

    - -c：产生tar打包文件
    - -v：显示信息
    - -f：指定文件名
    - -z：压缩
    - -x：解压
    - tar -xzvf xx.tar.gz -C /opt/
- file能探测文件的内部，知晓文件的类型

- 查看进程ps，默认情况下ps显示运行在当前控制台下的属于当前用户的进程

    - n -A，显示所有进程
    - n -N，显示与指定参数不符的所有进程
    - n -a，显示除控制进程和无终端进程外的所有进程
    - n -d，显示除控制进程外的所有进程
    - n -e，显示所有进程
    - n -C cmdlist，显示包含挂在cmdlist列表中的进程
    - n -G grplist，显示组ID在grplist列表中的进程
    - n -U userlist，显示属主的用户ID在userlist列表中的进程
    - n -l，显示长列表
    - n -f，显示完整格式输出
- ps -ef,ps -l,ps --forest

- top，实时显示进程

- kill pid，-s强终止

- killall 进程名

- mount 输出当前系统挂载的设备

- mount –t type directory，type指定了磁盘被格式化的文件系统，该命令需要root权限

- vfat-Windows长文件系统

- ntfs-高级文件系统

- iso9660-标准CD-ROM文件系统

- mount参数

    - ro：以只读形式挂载
    - rw：读写
    - user：允许普通用户挂载文件系统
    - check=none：挂载文件系统的时候不进行完整性检查
    - loop：挂载一个文件
- umount：卸载，卸载的时候提示系统可用lsof /path/device/node获取它的进程信息

- df：可以查询已挂载磁盘的使用情况，-h参数可以按照用户易读的形式显示

- du：会显示当前目录下所有的文件、目录和子目录的磁盘使用情况

    - n -c：显示所有已列出文件总的大小
    - n -h：易读
    - n -s：显示每个输出参数的总计
- sort：排序le

    - b：排序时候忽略起始的空白
    - C：不排序，如果数据无序也不要报告
    - c：不排序，但检查输入数据是不是已排序；未排序报告
    - d：仅考虑空白和字母，不考虑特殊字符
    - f：默认情况下，会将大写字母排在前面；这个参数会忽略大小写
    - g：按通用数值来排序，把值当做浮点数
    - i：在排序时忽略不可打印字符
    - k pos1 pos2：排序从pos1位置开始，如果指定了pos2，到pos2位置结束
    - M：按照三字符月份名按月份排序
    - m：将两个已排序的数据文件合并
    - n：按字符串数值来排序，不转换为浮点数
    - o file：将排序结果写出到指定的文件中
    - R file：按随机生成的散列表的键值排序
    - r：反序排序
    - S size：指定使用内存的大小
    - s：禁用最后重排序比较
    - T dir：指定一个位置来存储临时工作文件
    - t sep：指定一个用来区分键位置的字符
    - u：和-c参数一起使用，检查严格排序；当不和-c一起使用，仅输出第一例相似的两行
    - z：用NULL字符作为行尾，而不是用换行符
- 搜索数据：grep pattern file

    - v：方向搜索，输出不匹配该模式的行
    - n：显示匹配模式的行所在的行号
    - c：只需要知道多少行匹配
    - e：若需要制定多个匹配模式，可用-e来制定每个模式：grep –e t –e f file
- 压缩数据

    - gzip：压缩文件，.gz
    - gzcat：用来查看压缩过的文本文件的内容
    - gunzip：用来解压文件
- 归档数据：tar –cvf –tf –xvf(.tar) -xzvf(.tgz)

    - A：将一个已有tar归档文件追加到另一个已有tar归档文件
    - c：创建一个新的tar归档文件
    - d：检查归档文件和文件系统的不用之处
    - r：追加文件到已有tar归档文件结尾
    - u：将比tar归档文件中已有的同名文件新的文件追加到该tar归档文件中
    - x：从已有tar归档文件中提取文件
    - C dir：切换到指定目录
    - f file：输出结果到文件或设备file
    - j：将输出重定向给bzip2命令来压缩内容
    - p：保留所有文件权限
    - v：在处理文件时显示文件
    - z：将输出重定向给gzip命令来压缩内容
- 进程列表：(pwd; ls; cd/etc; echo $BASH_SUBSHELL)，若echo命令返回零，则表示没有子shell，若大于零，则表明存在shell。{}这个花括号不会创建子shell而[]会

- sleep 10&:睡眠10s

- jobs：显示当前运行在后台的作业 jobs -l还可显示命令的PID

- 后台运行：(tar *)&

- 协程：在后台生成一个shell，并在这个shell中执行命令。coproc My_job { sleep 10; }


## 组管理

### 基本介绍

​ 文件：所有者、所在组、其他组

- 改变文件所有者：chown 用户名 文件名
- 组的创建：groupadd 组名
- 修改文件所在组：chgrp 组名 文件名
- 其他组：排除所有者和所在组
- 修改用户所在组：usermod -g 组名 用户名
- 修改用户登录目录：usermod -d 目录名 用户名 改变该用户登录的初始目录

### 权限基本介绍

- rwx[421]

### 权限管理

- chmod：chmod u=rwx,g=rx,o=x 文件名
    - u、g、o、a（所有）
    - chmod 751 文件名
    - -R 递归修改
- chown
    - chown -R gemsuser /
    - chown newowner:newgroup file
- chgrp

## 定时任务调度

- crond任务调度
    - -e：编辑
        - 第几分钟 0~59
        - 第几小时 0~23
        - 第几天 1~31
        - 第几月 1~12
        - 星期几 0~7（0和7都代表星期日）
        - *任何时间
        - ,不连续时间
        - -范围
        - */n,每隔多久
    - -l：显示
    - -r：删除
    - service crond restart：重启任务调度

## 磁盘分区挂载

#### Linux分区

- mbr
- gtp

#### 硬盘说明

- IDE：hdx hda3-第一个IDE银盘上的第三个分区
- SCSI：sdx

lsblk -f：查看系统分区和挂载的磁盘

#### 挂载经典案例

1. fdisk /dev/sdb
2. mkfs - t ext4 /dev/sdb1
3. mount /dev/sdb1 /home/newdisk（临时挂载）
4. 永久挂载：vi /etc/fstab
   mount -a及时生效

#### 磁盘情况查询

- df -hl
- du -h /目录
    - -a含文件
    - --max-depth=1子目录深度
        - 统计文件个数 ls -l /home|grep “^-”|wc -l
        - 统计目录个数 ls -l /home|grep “^d”|wc -l
        - 统计文件个数包括子目录ls -lR /home|grep “^-”|wc -l
        - 以树状图 tree

## 网络配置

- ifconfig
- ping
- 指定固定ip：vi /etc/sysconfig/netwok-scripts/ifconfig-eth0,static,dns,gateway,onboot=yes
- server network restart

#### 修改主机名

- hostname
- vim /etc/sysconfig/network:HOSTNAME:yourname
- vim /etc/hosts

## 进程管理

#### 显示系统执行的进程

- ps
    - -a：显示当前终端所有的进程
    - -u：以用户的格式显示进程信息
    - -x：显示后台进程运行的参数
    - -ef：查看父进程

#### 终止进程

- kill
    - -9
- killall
    - killall gedit
- pstree
    - -p：显示进程的PID
    - -u：显示所属的用户

## 服务管理

### 管理指令

- service
    - service sshd status
- /etc/init.d：列出系统那些服务
- vi /etc/inittab：查看或者修改运行级别
- chkconfig：可以给每个服务的各个运行级别的自启动
    - chkconfig --list|grep sshd
    - chkconfig --level 5 sshd off

### 动态监控进程

- top
    - -d:top -d 10刷新时间
    - -i:
    - -p:
    - u：查看特定用户
    - k：进程号，杀进程
    - P：cpu排序
    - M：按内存排序
    - N：pid排序

### 监看网络情况

- netstat
    - -an：按一定排序排列输出
    - -p：显示那个进程在调用
    - netstat -anp|more
    - netstat -tunlp

## RPM和YUM

### RPM

readhat package manager

- rpm -qa|grep xx
- rpm -q foxmail
- rpm -qi foxmail
- rpm -ql foxmail：查看安装位置
- rpm -qf /etc/passwd：查询文件属于哪个包
- rpm -e RPM包名称：卸载
- rpm -e -nodeps：强制删除
- rpm -ivh foxmail
    - -i：install
    - -v：verbose提示
    - -h：hash进度条

### yum

- yum list|grep xx
- yum install xx

## 内建命令

### 外部命令

- which ps
- type -a ps

当外部命令执行时，会创建出一个子进程。这种操作成为衍生(forking)

### 内建命令

​ 不需要子进程来执行，如cd、exit。命令有多种形式，要查看命令的不同实现，使用type命令的-a选项，which只显示外部命令文件

- history：查看历史命令
- ！！：会从shell历史记录中换回的命令，然后执行该命令
- history -a：在打开的会话向.bash_history添加记录
- history -n：要强制读取.bah_history文件，可以使用history -n命令
- ！n：可用于执行该行指令
- alias -p：查看当前可用别名
- alias li=’ls -li’;

## 环境变量

### 全局环境变量

- printenv HOME
- env $HOME
- 创建全局变量：my_variable = ”I am Global now”; export my_variable
- 子shell无法使用export改变父shell中全局变量的值

### 局部环境变量

- set：显示特定进程设置的所有环境变量，包括局部变量、全部变量和用户定义变量
- echo $my_variable; my_variable=”hello world‘’

### 删除环境变量

- unset my_variable

### 设置PATH环境变量

- PATH=$PATH:dir,对PATH的修改只能持续到推出或者重启系统

### 永久改变环境变量

- 登录shell
    - /etc/profile
    - $HOME/.bash_profile
    - $HOME/.bashrc
    - $HOME/.bash_login
    - $HOME/.profile
    - $HOME下，shell会按照以上黄色标记顺序执行，运行第一个找到的文件，其余会被忽略
- 交互式shell：不是登录系统时启动的，检查HOME目录下的.bashrc文件
- 非交互式shell：BASH_ENV环境变量

### 数组变量

- mytest=(ont two three)

- echo ${mytest[*]}

- **获取数组长度**

    - arr_length=${#arr_number[*]}或${#arr_number[@]}均可，即形式：${#数组名[@/*]}
- **读取某个下标的值**

    - arr_index2=${arr_number[2]}，即形式：${数组名[下标]}
- **对某个下标赋值**

    - 如果该下标元素已经存在，会修改该下标的值为新的指定值。
    - 如果指定的下标已经超过当前数组的大小，新赋的值被追加到数组的尾部。
- **删除操作**

    - 清除某个元素：unset arr_number[1]，这里清除下标为1的数组；
    - 清空整个数组：unset arr_number;
- **分片访问**

    - 分片访问形式为：${数组名[@或*]:开始下标:结束下标}，注意，不包括结束下标元素的值。
- **模式替换**

    - 形式为：${数组名[@或*]/模式/新值},例如：${arr_number[@]/2/98}
- **数组的遍历**

  ```shell
  for v in ${arr_number[@]}; do
  　　　　　echo $v;
  　　done
  ```


## 文件权限

### /etc/passwd

- 登录用户名
- 用户密码
- 用户账户的UID(数字形式)
- 用户账户的组ID(GID)(数字形式)
- 用户账户的文本描述(称为备注字段)
- 用户HOME目录的位置
- 用户的默认shell

### /etc/shadow

- 与/etc/passwd文件中的登录名字段对应的登录名
- 加密后的密码
- 自上一次修改密码后过去的天数密码（自1970年1月1日开始计算）
- 多少天数才能更改密码
- 多少天后必须修改密码
- 密码过期前提前多少天提醒用户更改密码
- 密码过期后多少天禁用用户密码
- 用户账户被禁用的日期
- 预留字段给将来使用

### 添加新用户

- /usr/sbin/useradd -D:显示默认值
    - 新用户会被添加到GID为100的公共组
    - 新用户的HOME目录将会位于/home/loginname
    - 新用户账户密码在过期后不会被禁用
    - 新用户账户未被设置过期日期
    - 新用户账户将bash shell作为默认shell
    - 系统会将/etc/skel目录下的内容复制到用户的HOME目录
    - 系统为该用户账户在mail目录下创建一个用于接收邮件的文件

​ 使用useradd命令默认不产生home目录。要在创建用户时改变默认值，可以使用命令行参数

- -c comment：给新用户添加备注
- -d home_dir
- -e expire_date
- -f inactive_days
- -g initial_group
- -G group …：指定用户除登录组之外所属的一个或多个附加组
- -k 必须和-m一起使用，将/etc/skel目录的内容复制到用户的HOME目录
- -m 创建用户的HOME目录
- -M 不创建用户的HOME目录（当默认设置里要求创建时才使用这个选项）
- -n 创建一个与用户登录名的新组-r 创建系统账户
- -p passwd
- -s shell
- -u uid

修改系统默认值，可以在-D选项后跟上一个指定的值来修改系统默认的新用户设置

### 删除用户

- userdel -r，会同时删除用户目录

### 修改用户

- usermod
    - -l：修改用户账户的登录名
    - -L：锁定账户，使用户无法登录
    - -p：修改账户的密码
    - -U：解除锁定，使用户能够登录
- passwd
- chpasswd：从文件中读取登录名密码对，并更新密码
- chage：修改密码的过期日期
- chfn：修改用户账户的备注信息
- chsh：修改用户账户的默认登录shell

### 使用Linux组

- /etc/group文件
    - 组名
    - 组密码
    - GID
    - 属于改组的用户列表、
- 创建新组：groupadd，创建新组时，默认没有用户被分配到改组。可用usermod命令
  usermod -G shared rich，-g指定的组名会替换掉该账户的默认组。-G将改组添加到用户的数组的列表里，不会影响默认值。
- 修改组：-g GID、-n 组名

### 理解文件权限

- -：代表文件
- d：代表目录
- l：链接
- c：字符型设备
- b：块设备
- n：代表网络设备
- r\w\x
- 对象的属主、对象的属组、系统其它用户
- 默认的文件权限：umask

### 改变权限

- chmod options mode file
    - u:用户
    - g：组
    - o：其它
    - a：代表上述所有
- chown：更改宿主
- sudo su登录高级管理员
- 关机，init 0（登录高级管理员）
- sudo init 0（正常用户）
- -h/--help，帮助文档
- ls - -help |more(翻页，spaces)
- CTRL+C，强制停止，可多次键入
- CTRL+D，退出当前用户（exit）
- CD + /,回到根目录
- CD，回到当前用户的根目录
- Find / -name “”,从根目录查找,find .当前目录，find ./当前目录下的所有目录
- Mkdir、rmdir、rm –rf
- cp –R 源文件 目标位置
- mv 移动、重命名
- cat 查看文件 cat doc |more
- tail 查看文件，从尾开始，默认后10行
- tail –f 实时滚动 tail –f user.log |grep “2018-04-15” |more
- head 查看文件，从头开始，默认头10行， head -20 doc
- df –h 查看磁盘情况
- du –sh 统计查看当前目录的使用情况du –sh *，详细
- alias eric =’ ls –l –a”，别名，unalias,取消别名
- ls -alh

## VI

三种操作模式：命令模式、输入模式、末行模式

- vi newfile
- q!
- wq!
    - X
- i
- o当前行下一行插入
- O 当前行的上一行插入
- x 删除当前字符
- h、l、k、j 左右上下
- ctrl+f，向下翻整页 u半页
- ctrl+b，向上翻整页 d半页
- shift+4，跳到行首
- shift+6，跳到行尾
- w，跳转到当前贯标位置所在的后一个单子的首字符，b-前，e-后一个单词的尾字符
- set nu,在编辑器中显示行号，set nonu
- gg 跳到文件顶部
- G 跳到文件尾部
- 数字+G 跳到某一行
- dd删除整行/剪切，6dd
- yy 赋值当前行，5yy
    - p 粘贴
- u 撤销当前操作
- U 取消对当前行的所有操作
- ctrl+r 对使用u命令撤销的操作进行恢复
- dw 删除当前字符到单词尾，包括空格，de不包括空格，yw,ye
- d+shifit+4 删除当前字符到行尾，d+shift+6到行首,y+shift+4,y+shift+6
- J删除行尾的换行符，相当于合并前行和后行
- 向下查找，下一个n
- ?向上查找，上一个N
- :s/old/new 将当前行的第一个字符old替换成new
- :s/old/new/g 将当前行的所有字符old替换成new
- :#,#s/old/new/g 在行号#,#范围内替换所有的字符串
- %s/old/new/g 全文范围内替换
- s/old/new/c 加上c表示替换前用户确认

## Touch命令

Touch [options] filename

- -a:改变访问时间
- -m:改变改动时间
- -t timestamp:改变范文时间和改动时间为timestamp

## Mkdir命令

Mkdir [options] dirname

- -p:递归创建文件夹：Mkdir –p zsy/hxn
- -mmode:新建文件夹，并设置文件夹的文件访问模式：mkdir –m770 zsy

## Rm命令

Rm [options] filename/dirname

- -f:强制删除
- -r:递归删除
- -i：删除前确认

## 构建基本脚本

### 创建shell脚本文件

- 在创建shell脚本文件时，必须在文件的第一行指定要使用的shell，格式为 #!/bin/bash

- testing = `date`与testing=$(date)

- 输出重定向>,>>追加

- 输入重定向<,<<

- expr，处理数学表达式或者$[operation ]，只能处理整数

- 浮点解决方案:bc

    - bc -q:不显示bash计算器冗长的欢迎信息

    - scale

    - 在脚本中使用bc：var1=$(echo “scale=4;3.44/5”|bc)

      ```shell
      n variable =$(bc<<EOF
       options
       statements
       expressions
       EOF
       )
      ```


### 退出脚本

- Linux退出状态码
    - 0：命令成功结束
    - 1：一般位置错误
    - 2：不适合的shell命令
    - 126：命令不可执行
    - 127：没找到命令
    - 128：无效的推出参数
    - 128+x：与Linux信号x相关的严重错误
    - 130：通过Ctrl+C终止的命令
    - 255：正常范围之外的退出状态码

## 结构化命令

### if-then

if后面的命令如果返回值为零，则执行then

```shell
if command
 then
   commands
 fi
if command; then
 commands
 fi
```

### if-then-else

```shell
if command
 then
  commands
 else
  commands
 fi
```

### test

- test condition
- 数值比较
    - -eq
    - -ge
    - -gt
    - -le
    - -lt
    - -ne
    - n 字符串比较,>或者<需要使用转义符
    - =
    - !=
    - <
    - -n str1:检查str1的长度是否非零
    - -z str1:检查str1的长度是否为
- 文件比较
    - -d file：检查file是否存在并是一个目录
    - -e file：检查file是否存在
    - -f file：检查file是否存在并是一个文件
    - -s file：检查file是否存在并非空
    - -r file：检查file是否存在并可读
    - -w file：可写
    - -x file：可执行
    - -O file：检查file是否存在并属当前用户素有
    - -G file：检查file是否存在并且默认组与当前数组先攻
    - file1 -nt file2：检查file1是否比file2新
    - file1 -ot file2：旧

### 复合条件测试

- [condition1 ] && [condition2]
- [condition1 ] || [condition2]

### if-then高级特性

- (( expression ))，使用高级数学表达式
    - val++
    - val--
    - ++val
    - --val
    - !：逻辑求反
    - ~：位求反
    - **
    - <<
    - />
    - &
    - |
    - &&
    - ||
- [[ expression ]],提供了针对字符串比较的高级特性，模式匹配

### case

```shell
case variable in
 pattern1 | pattern2) commands1;;
 pattern3) commands2;;
 *) default commands;;
 esac
```

### for

```shell
for var in list
 do 
   commands
 done
```

### 字符串拼接

list = $list” Connecticut”:用于拼接

### 更改字符串分隔符

- IFS=$’\n’
- 可临时修改分隔符代码：
  IFS.OLD=$IFS
  IFS=$’\n’
  <在代码中使用新的分隔符>
  IFS=$IFS.OLD

### 通配符读取目录

- for file in /home/rich/test/*
- if [-d “$file” ]:使用双引号可以支持目录中有空格

### C语言风格的for命令

```shell
for(( i=1; i<=10;++ ))
 do 
  statement
 done
```

### while命令

```shell
while test command
 do
  other commands
 done
```

### until命令

```shell
until test commands
 do
  other commands
 done
```

### break

- break
- break n;1-当前循环；2-停止下一级的外部循环

### continue

- continue
- continue n

### read

- -a 后跟一个变量，该变量会被认为是个数组，然后给其赋值，默认是以空格为分割符。
- -d 后面跟一个标志符，其实只有其后的第一个字符有用，作为结束的标志。
- -p 后面跟提示信息，即在输入前打印提示信息。
- -e 在输入的时候可以时候命令补全功能。
- -n 后跟一个数字，定义输入文本的长度，很实用。
- -r 屏蔽\，如果没有该选项，则\作为一个转义字符，有的话 \就是个正常的字符了。
- -s 安静模式，在输入字符时不再屏幕上显示，例如login时输入密码。
- -t 后面跟秒数，定义输入字符的等待时间。
- -u 后面跟fd，从文件描述符中读入，该文件描述符可以是exec新开启的。
- read -r userid name:自动读取下一行，读取完返回FALSE

## 处理用户输入

### 命令行参数

$0,$1,${10}

### 读取脚本名

name =$(basename $0)

### 参数统计

- $#:命令行参数个数
- ${!#}：返回最后一个变量名
- $*：将变量当成单个参数
- $@：会单独处理每个参数
- shift：左移，$1会丢失，$0不变
- shift n
- #：去掉左边
- %：去掉右
- 单一符号是最小匹配，两个符号是最大匹配
- ${file:0:5}：提取最左边的5个字
- 对变量值的字符串做替换：
    - ${file/dir/path}：将一个dir替换为path
    - $(file//dir/pah)：将全部dir替换为path
    - ${file#*/}：删掉第一个 / 及其左边的字符串*
    - *${file##*/}：删掉最后一个 / 及其左边的字符串
    - ${file#*.}：删掉第一个 . 及其左边的字符串*
    - *${file##*.}：删掉最后一个 . 及其左边的字符串
    - ${file%/*}：删掉最后一个 / 及其右边的字符串*
    - *${file%%/*}：删掉第一个 / 及其右边的字符串
    - ${file%.*}：删掉最后一个 . 及其右边的字符串*
    - *${file%%.*}：删掉第一个 . 及其右边的字符串
- $$：本身的PID
- $!：shell运行的最后的后台process的id
- $*：”$1 $2 …$n”输出所有的参数
- $@：”$1” … “$n”
- $#：参数个数

## Debug

### Segmentation fault(core dumped)

- 在gcc编译的程序时使用”-g”选项，使编译文件带有调试信息
- 检查Linux设置的在发生段错误时文件大小
    - ulimit -a
- 默认不产生，取消该限制
    - ulimit -c unlimited
- 用gdb分析core文件
- gdb {executable} {dump file
- bt：查看出错的堆栈(backtrace)
- frame {num}：分析每个栈桢中变量情况
- info locals：查看局部变量
- info args：查看参数
- where：查看调用参数

## Nmon监控工具

### 非交互模式

- -f：切换到非交互模式，必须是第一个参数
- -s：每多少秒采集一次数据
- -c：总共采集多少次数
- -t：包括Top进程的状态
- -m：指定生成的文件目录

可用于生成尾号为.nmon的CSV文件，可使用nmon_analyser把nmon生成的文件转换为excel。

http://www.ibm.com/developerworks/wikis/display/Wikiptype/nmonanalyser