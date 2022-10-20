# VIM Notes

## terminal

### vim open multi windows and files

```bash
vim file1 file2
vim -On file1 file2 # 垂直打开
vim -on file1 file2 # 水平打开
vimdiff file1 file2
```

### only read mode

```bash
view file1
```

## normal mode

### move cursor

```bash
h/j/k/l
w/W/b/B
e/E/ge/gE
0            # 光标移到行首
$            # 光标移到行末
%            # 光标跳到括号匹配处
''           # 光标回到上一次的位置
[[           # 跳转至上一个函数
]]           # 跳转至下一个函数
```

### move screen

```bash
zz           # 将当前行移动到屏幕的中间
zt           # 将当前行移到屏幕顶部
zb           # 将当前行移到屏幕底部
```

### switch insert mode

```bash
i/I/a/A/o/O  # switch insert mode
v/V          # switch view mode
```

### handle file

```bash
ZZ           # 保存并退出
ZQ           # 不保存退出
gf           # 打开头文件
gt           # goto next tab
gT           # goto prev tab
```

### usually used

```bash
r/R          # replace
x            # delete char
dd           # delete line
dw/dW/de/dE  # delete word/WORD/"word of end"/"WORD OF END"
D            # 删除光标之后的字符串
d0           # 删除光标之前的字符串
~            # 大小写转换
gUU          # 将当前行转化为大写
guu          # 将当前行转化为小写
gUw          # 将单词从小写转化为大写
u            # 撤销
J            # 去掉该行的换行符


/\cv_mov_b32_e32    # 忽略大小写
/[char]\+           # 匹配多个字符
```

### Keyboard Shortcuts

```bash
<C-e> / <C-y> # 屏幕向下(上)移动一行
<C-w-w>   # switch window
<C-w-r>   # swap window

<C-a>   # add 1
<C-x>   # sub 1
<C-V> + g + <C-a> # add 1,2,3,4,5...
<C-V> + g + <C-x> # sub 1,2,3,4,5...
<C-r>   # 撤销回退
<C-n>   # 代码补全
<C-w-s>   # 水平新建窗口
<C-w-v>   # 垂直新建窗口
<C-g>   # 显示文件信息
<C-o>   # 回到上次打开的文件
```

## insert mode

```bash
:e file2  # 当前窗口打开
:sp file2  # 水平切分窗口
:vsp file2  # 垂直切分窗口

:ls   # list open files
:bn   # switch file-n

:e   # 重新加载文件
:e!   # 放弃当前修改重新加载文件

# tabe相关
:tabe file...  # new table file...

:verbose set mouse # 查看最后在哪里配置的mouse
:w ! sudo tee %  # 强制保存

:vert diffsplit file2 # 比较两个文件不同

:5,15s/dog/cat/g # 替换行内字符串
:%s/dog/cat/g
:%s/from/to/gc

:m,n>   # 向>缩进
:m,n<   # 向<缩进
:%retab!  # tab <-> 空格
```

### tab <-> space

```bash
# TAB替换为空格：
:set ts=4
:set expandtab
:%retab!

# 空格替换为TAB：
:set ts=4
:set noexpandtab
:%retab!
```

### 设置空格或回车可见

```bash
:setlocal list
:set listchars=tab:>~,trail:.
```

## visual mode

> [!TIP]
> ``<C-v> select chars, <yy> to copy, and then use <C-v> and <p> to put it.``

### plugins

#### 折叠相关

```bash
z f %  # 折叠 fold %
z o   # 展开 open
z c   # 折叠 close
```

#### vim-markdown

- zr: reduces fold level throughout the buffer
- zR: opens all folds
- zm: increases fold level throughout the buffer
- zM: folds everything all the way
- za: open a fold your cursor is on
- zA: open a fold your cursor is on recursively
- zc: close a fold your cursor is on
- zC: close a fold your cursor is on recursively

### cscope

```bash
cscope -Rbq
file cscope.out
:cs add {file|dir} [pre-path] [flags]
cs find {querytype} {name}
#0或s：查找这个(指name参数，下同)C符号。
#1或g：查找这个定义。
#2或d：查找被这个函数调用的函数。
#3或c：查找调用该函数的函数。
#4或t：查找这个文本字符串。
#6或e: 查找这个egrep的pattern。
#7或f：查找这个文件。
#8或i：查找#include了这个文件的所有文件。
```

### gitgutter

> [c
>
> ]c
>
> :GitGutterToggle
>
> :GitGutterDiffOrig
>
> :GitGutterQuickFix
>
> :copen
