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
> \	：转义字符
> ‘’	：转义所有变量变为字符串
> ""	: 不转义
> ``	：命令执行后的返回结果

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


