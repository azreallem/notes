# GDB Notes

## notes

```bash
<C-L>                   # clear screen
(gdb) p /x *p@100       # print array of length
```

## terminal

```bash
# gdb qemu
gdb attatch 243591

# gdb vmlinux
gdb --args ... \
-gdb tcp::2730 \
-S

# gdb set env
env -i ... \
gdb

# `objdump` and `gcc compile of static`
~/work/toolchain/install/bin/loongarch32-linux-gnu-objdump -d vmlinux > a.txt
~/work/toolchain/install/bin/loongarch32-linux-gnu-gcc fork.c -o init --static
```

```bash
(gdb) target remote : 2730

(gdb) show env

(gdb) b *0xa09d4000
(gdb) tb a.c:15
(gdb) break … if cond
(gdb) b 10 if i==101

(gdb) watch env->CSR_CRMD
(gdb) info watchpoints

(gdb) c

(gdb) call func

(gdb) info register
(gdb) info b
(gdb) info 

(gdb) print

# 查看内存地址中变量
(gdb) x ...

# 断点
(gdb) disable 1 
(gdb) enable 1

# print
(gdb) p /x ...
(gdb) p /x *...
(gdb) print *(int *)point@10

(gdb) info reg
(gdb) x /32i 0x00fd0000
(gdb) si

(gdb) moni log mem
(gdb) moni log int

(gdb) bt

# list
(gdb) l


(gdb) layout n
#<C-L>   刷新窗口
#<C-x-a> 回到传统模式，即退出layout，回到执行layout之前的调试窗口。
(gdb) fs n
(gdb) winheight src -5
(gdb) winheight asm -5
(gdb) winheight cmd -5
```

> s 相当于其它调试器中的“Step Into (单步跟踪进入)”；
>
> n 相当于其它调试器中的“Step Over (单步跟踪)”
>
> si命令类似于s命令，ni命令类似于n命令。所不同的是，这两个命令（si/ni）所针对的是汇编指令，而s/n针对的是源代码。

常见的输出格式：

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
```

## Tips

### `until [Num]`

(gdb) continue run until 到 Num 行；

```bash
(gdb) until 1735
```

### `watch [variate]` and  `ignore [No. of breakpoints] [time]`

```bash
(gdb) watch Index
(gdb) ignore [No] 5
(gdb) c
```

**For example**:

```c
(gdb) until 1731
(anonymous namespace)::MPPassManager::runOnModule (this=0x120125810, M=...)
    at /home/loongson/work/gpu/llvm-toolchain-8-8.0.1/llvm/lib/IR/LegacyPassManager.cpp:1731
1731      for (unsigned Index = 0; Index < getNumContainedPasses(); ++Index) {
(gdb) watch Index
Hardware watchpoint 6: Index
(gdb) ignore 6 10
Will ignore next 10 crossings of breakpoint 6.
(gdb) c
Continuing.

Thread 1 "tutorial01_firs" hit Hardware watchpoint 6: Index

Old value = 9
New value = 10
0x000000fff1455b14 in (anonymous namespace)::MPPassManager::runOnModule (this=0x120125810, M=...)
    at /home/loongson/work/gpu/llvm-toolchain-8-8.0.1/llvm/lib/IR/LegacyPassManager.cpp:1731
1731      for (unsigned Index = 0; Index < getNumContainedPasses(); ++Index) {
(gdb) n
1732        ModulePass *MP = getContainedPass(Index);
(gdb) p Index
$63 = 10
(gdb) ignore 6 3
Will ignore next 3 crossings of breakpoint 6.
(gdb) c
Continuing.

Thread 1 "tutorial01_firs" hit Hardware watchpoint 6: Index

Old value = 13
New value = 14
0x000000fff1455b14 in (anonymous namespace)::MPPassManager::runOnModule (this=0x120125810, M=...)
    at /home/loongson/work/gpu/llvm-toolchain-8-8.0.1/llvm/lib/IR/LegacyPassManager.cpp:1731
1731      for (unsigned Index = 0; Index < getNumContainedPasses(); ++Index) {
(gdb) n
1732        ModulePass *MP = getContainedPass(Index);
(gdb) p Index
$64 = 14
(gdb) 
```
