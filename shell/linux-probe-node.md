# Linux 就该这么学

## 第2章 新手必须掌握的 Linux 命令

### EASY COMMAND
```bash
echo [STRING]
date
reboot
poweroff
wget http://www.baidu.com
top
pidof [program]
kill -9 [pid]
killall [program]
ps -aux
```

### 系统状态检测命令
```bash
ifconfig
ip addr
uname -a
uptime 		# 查看信息的负载情况
free -h		# 查看内存的使用量信息
whoami
last		# 查看所有系统的登录记录
history [-c]	# -c clear history
```

### 工作目录切换命令
```bash
pwd
cd
ls -lt		# 时间从大到小排序
ls -ltr		# 时间从小到大排序
ls -ld		# 显示目录属性信息
ls -alhtr
cat [filename]
cat [filename] | less
cat [filename] | more
cat > 1.txt << EOF
EOF
head -n 20 [filename]
tail -f [filename]		# 实时刷新
cat [filename] | tr [a-z] [A-Z] # 文本替换
wc -lwc	[filename]		# 统计文件的行数，字数，字节数
stat [filename]			# 查看文件详情
cut -d: -f1 /etc/passwd		# 按列提取，分隔符:
diff [-c] [filename1] [filename2]
```

### 文件目录管理命令
```bash
touch -d "2017-05-04 01:12" [filename]	# 修改文件日期
					# -a 读取日期
					# -m 修改日期
					# -d 同时修改读取日期和修改日期
mkdir -p a/b/c/d
cp [-p|-d|-r|-i|-a] [file1] [file2]	# -p 保留原始文件属性
					# -d 若对象为链接文件，则保留为链接文件
					# -r 递归复制
					# -i 询问是否覆盖
					# -a 相当于-pdr
mv [file1] [file2]
rm -rf *
dd if=/dev/zero of=560_file count=1 bs=560M
dd if=/dev/cdrom of=file_name.iso
tar -cvf [new_filename] [dir]
tar -xvf [filename]
tar -t [filename]			# 查看压缩包有哪些文件
grep -Iinr "string" [filename]		# 查找关键字
grep -v "string" [filename]		# 去掉string的行
find ./ -name "1.txt"
```

## 第3章 管道符、重定向与环境变量
### 重定向
- 标准输入重定向（STDIN，文件描述符0）: 键盘、文件、命令中输入
- 标准输出重定向（STDOUT，文件描述符1）：输出到屏幕
- 错误输出重定向（STDERR，文件描述符2）：输出到屏幕
```bash
ls xxx 1.txt >> 1.txt 2>&1		# 正确、错误都输出到文件1.txt
ls xxx 1.txt &>> 1.txt

ls xxx 1.txt 2>> 1.txt			# 仅输出错误到文件1.txt
```

### 管道
命令1 ｜ 命令2 ｜ 命令3

### 命令行通配符
```bash
ls *.txt
ls 1.tx?
ls [0-9].txt
```

### 转义字符
- \	：转义字符
- ‘’	：转义所有变量变为字符串
- ""	: 不转义
- \`\`	：命令执行后的返回结果  

### 环境变量
- HOME
- SHELL
- HISTSIZE
- HISTFILESIZE
- MAIL
- LANG
- RANDOM
- PS1
- PATH
- EDITOR

## 第4章 Shell命令脚本
### shell传参
例如`$0` 对应的是当前 Shell 脚本程序的名称，`$#`对应的是总共有几个参数，`$*`对应的是所有位置的参数值，`$?`对应的是显示上一次命令的执行返回值，而`$1`、`$2`、`$3`......则分别对应着第 N 个位置的参数值。

```bash
#!/bin/bash
echo "当前脚本名称为$0"
echo "总共有$#个参数，分别是$*"
echo "第1个参数是$1,第5个参数是$5"
```

### 判断参数
**条件表达式两边有一个空格**

- 文件测试语句
- 逻辑测试语句
- 整数测试语句
- 字符串测试语句

| 运算符 | 作用 |
| ------ | ---- |
| -d | 测试文件是否为目录类型 |
| -e | 测试文件是否存在 |
| -f | 判断是否为一般文件| 
| -r | 测试当前用户是否有权限读取 |
| -w | 测试当前用户是否有权限写入 |
| -x | 测试当前用户是否有权限执行 |

> 0: 是， 1: 不是  
> 与C语言不同，C语言0代表false，非0代表true

- &&：表示当前指令执行成功后才会执行；
- ||：表示当前指令执行失败后才会执行；

```bash
#!/bin/bash
[ -d $1 ]
echo “$1是否为目录（0:是，1:不是）：$?”
[ -e $2 ] && echo "$2文件存在" || "$2文件不存在"
[ -f $2 ] && echo "$2为一般文件"
[ -r $3 ] && echo "当前用户对$3有read权限"
[ -w $4 ] && echo "当前用户对$4有write权限"
[ -x $4 ] && echo "当前用户对$4有execute权限"
```

| 运算符 | 作用 |
| ------ | ---- |
| -eq | 是否等于 |
| -ne | 是否不等于 |
| -gt | 是否大于greater |
| -lt | 是否小于lesser |
| -le | 是否小于等于lesser equal |
| -ge | 是否大于等于greater equal |

```bash
#!/bin/bash
free -m | grep Mem: | awk '{print $4}'
FreeMem=$?
[ $FreeMem -lt 1024 ] && echo "Insufficient Memory"
```

> notes: `awk \`{print $4}\``表示只保留第4列，`FreeMem=$?`赋值语句等号左右两边没有空格。

| 运算符 | 作用 |
| ------ | ---- |
| = | 比较字符串内容是否相同 |
| != | 比较字符串内容是否不同 |
| z | 比较字符串内容是否为空 |

```bash
#!/bin/bash
[ -z $1 ] && echo "参数1为：$1" || echo "没有字符串传参"
[ $1 == $2 ] && echo "$1 == $2" || echo "$1 != $2"
```

### 流程语句
#### if 条件测试语句
```bash
if [ ... ]
then
......
elif [ ... ]
then
......
else
......
fi
```

```bash
#!/bin/bash
read -p "Enter your score (0-100):" GRADE
if [ $GRADE -ge 85 ] && [ $GRADE -le 100 ]
then
    echo "$GRADE is Excellent"
elif [ $GRADE -ge 70 ] && [ $GRADE -lt 85 ]
then
    echo "$GRADE is Pass"
else
    echo "$GRADE fall"
fi
```

#### for 条件循环语句
```bash
for ... in ...
do
......
done
```

```bash
#!/bin/bash
read -p "Enter The Users Password : " PASSWD
for UNAME in `cat users.txt`
do
id $UNAME &> /dev/null
if [ $? -eq 0 ]
then
echo "Already exists"
else
useradd $UNAME &> /dev/null
echo "$PASSWD" | passwd --stdin $UNAME &> /dev/null
if [ $? -eq 0 ]
then
echo "$UNAME , Create success"
else
echo "$UNAME , Create failure"
fi
fi
done
```

#### while 条件循环语句
```bash
while ...
do
......
done
```

```bash
#!/bin/bash
PRICE=$(expr $RANDOM % 1000)
TIMES=0
echo "商品实际价格为 0-999 之间，猜猜看是多少?"
while true
do
read -p "请输入您猜测的价格数目:" INT
let TIMES++
if [ $INT -eq $PRICE ]
then echo "恭喜您答对了，实际价格是 $PRICE" echo "您总共猜 g $TIMES 次"
exit 0
elif [ $INT -gt $PRICE ]
then echo "太高了!"
else
echo "太低了!"
fi
done
```

#### case 条件测试语句
```bash
case ... in
[a-z][A-Z])
.......
;;
[0-9])
.......
;;
*)
.......
esac
```

```bash
#!/bin/bash
read -p "请输入一个字符，并按 Enter 键确认:" KEY
case "$KEY" in
[a-z]|[A-Z])
echo "您输入的是 字母。"
;;
[0-9])
echo "您输入的是 数字。"
;;
*)
echo "您输入的是 空格、功能键或其他控制字符。"
esac
```



