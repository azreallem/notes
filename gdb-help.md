# GDB Notes

## notes

```text
<C-L>                   # clear screen
(gdb) p /x *p@100       # print array of length
```

## terminal

```bash
# gdb qemu
gdb attatch 243591

# gdb vmlinux
gdb --args ...
-gdb tcp::2730
-S

# gdb set env
env -i ... \
gdb
```

```Bash
target remote : 2730

show env

b *0xa09d4000
tb a.c:15
break … if cond
b 10 if i==101

watch env->CSR_CRMD
info watchpoints

c

call func

info register
info b
info 

print

# 查看内存变量
x ...

# 断点
disable 1 
enable 1

# print
p /x ...
p /x *...
print *(int *)point@10

info reg
x /32i 0x00fd0000
si
...

moni log mem
moni log int

bt

./work/toolchain/install/bin/loongarch32-linux-gnu-objdump -d vmlinux > a.txt
~/work/toolchain/install/bin/loongarch32-linux-gnu-gcc fork.c -o init --static

# list
l

layout n
#<C-L>   刷新窗口
#<C-x-a> 回到传统模式，即退出layout，回到执行layout之前的调试窗口。
fs n
winheight src -5
winheight asm -5
winheight cmd -5
```

> s 相当于其它调试器中的“Step Into (单步跟踪进入)”；
>
> n 相当于其它调试器中的“Step Over (单步跟踪)”
> si命令类似于s命令，ni命令类似于n命令。所不同的是，这两个命令（si/ni）所针对的是汇编指令，而s/n针对的是源代码。

常见的输出格式

- x: 十六进制格式
- d：有符号的十进制整数格式
- u：无符号的十进制整数格式
- o：八进制整数格式
- t：二进制整数格式
- c：字符格式
- f：浮点数格式

## thread

```bash
info thread
thread ID
bt
s / n
```
