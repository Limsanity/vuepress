---
sidebar: auto
title: 'Linux鸟哥私房菜'
---

# Linux鸟哥私房菜

## 第10章

### 认识Bash这个shell

#### Bash的功能

- 历史命令
  - `~/.bash_history`中存储着历史命令，可能存在黑客入侵，利用该命令查找到系统相关的操作，例如：在命令行中直接输入MySQL密码。 
- 命令别名设置功能
  - `alias lm='ls -al'`
  - `unalias lm`

#### 查询命令是否为Bash shell的内置命令

```bash
type [-tpa] name
```

#### 命令的执行与快速编辑按钮

```bash
cp /var/spool/mail/root \
> /etc/fstab/root
```

需要注意上述命令中的符号`\`，其后面没有空格，如果加上了空格，则反斜杠将转义空格键而不是Enter键。

如果命令行太长并且需要删除时：

- ctrl + u：从光标处向前删除
- ctrl + k：从光标处向后删除
- ctrl + a：将光标移动到最前面
- ctrl + e：将光标移动到最后面

### Shell的变量功能

#### 变量的使用与设置：echo、变量设置规则、unset

当变量被使用时，前面必须加上美元符号`$`。

显示出环境变量HOME：

```bash
echo $HOME
```

设置变量：

```bash
myname=VBird
echo ${myname} # or echo $myname
```

变量设置规则：

- 等号两边不能直接加空格，错误示例：`myname=Vbird Tsai`。
- 变量只能是英文字母和数字，且开头不能是数字。
- 变量内容若有空格，可使用单引号或双引号将变量内容结合起来，但：
  - 双引号内的特殊字符如`$`等，可以保持原有特性，例如：`var = "langis $LANG"`，使用echo输出`var`得到的内容则为`lang is zh_CN.UTF-8`。
  - 单引号内的特殊字符则仅为一般字符。
- 可用转义字符`\`将特殊符号变为一般字符。
- 在一串命令执行中，还需要借由其他额外的命令所提供的信息时，可以使用反单引号或`$(命令)`。例如：`version=$(uname -r)`。
- 若该变量需要在其他子程序执行，则需要用export来使其变成环境变量。
- 通常大写变量为系统默认变量，小写变量为自行设置的变量。
- 取消变量的方法为unset。`unset myname`。

#### 环境变量的功能

- 用`env`查看环境变量
- 用`set`查看所有变量
- `?`是上一个执行命令的返回值
- `PS1`是提示字符的设置

#### 影响显示结果的语系变量

#### 变量的有效范围

#### 变量键盘读取、数组与声明：read、array、declare

- read

```bash
read [-pt] variable

-p: 后面可以接提示字符
-t: 后面可以接等待的秒数

read -p "Please keyin your name: " -t 30 name # example
```

- declare, typeset

```bash
declare [-aixr] variable
-a: 将变量定义为数组类型
-i: 将变量定义为整数类型
-x: 将变量变成环境变量
-r: 将变量变为readonly类型，并且不能被unset

declare -i sum=100+300+50
echo ${sum} # 450

declare +x sum # 使用+号可以取消操作

declare -a var
var[1]=lim
echo $var[1] # lim
```

#### 与文件系统及程序的限制关系：ulimit

```bash
ulimit [-SHacdfltu] [配额]
-f: 此shell可以建立的最大文件容量，单位为KB

ulimit -f 10240 # 只能建立10MB以下的文件
```

#### 变量内容的删除、取代与替换

- `#`：符合替换文字的最短的那一个
- `##`：符合替换文字的最长的那一个
- `%`：从后往前替换最短的一个
- `%%`：从后往前替换最长的一个
- `/` ：替换第一个字符串
- `//`：替换所有字符串

```bash
$ path=${PATH}
$ ehco ${path}
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin

$ echo ${path#/*local/bin:}
/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin

$ echo ${path##/*}
/home/dmtsai/bin

$ echo ${path%:*bin}
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin

$ echo ${path%%:*bin}
/usr/local/bin

$ echo ${path/sbin/SBIN}
/usr/local/bin:/usr/bin:/usr/local/SBIN:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin

$ echo ${path//sbin/SBIN}
/usr/local/bin:/usr/bin:/usr/local/SBIN:/usr/SBIN:/home/dmtsai/.local/bin:/home/dmtsai/bin
```

- 使用`-`测试变量是否存在
- 使用`:-`设置不存在或者空变量
- `+`与`-`相反
- `:+`与`:-`相反
- 还有`=`、`:=`、`?`、`:?`
- `=`会影响旧变量的内容
- 当旧变量不存在时`?`会报错

```bash
$ echo ${username}
     # 出现空白，变量可能为空或者不存在
$ username=${username-root}
root # 因为没有设置username，所以主动给予内容root
$ username="lim"
$ username=${username-root}
lim  # 因为设置了username，所以不会替换
$ username=""
$ username=${username:-root}
root # :-会替换空字符变量
```

### 命令别名与历史命令

