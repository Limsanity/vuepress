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