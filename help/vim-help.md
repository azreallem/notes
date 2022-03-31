# bash
## vim打开多窗口、多文件
```bash
vim file1 file2
vim -On file1 file2	# 垂直打开
vim -on file1 file2	# 水平打开
vimdiff file1 file2
```

## only read mode 
```bash
view file1
```

# normal mode
```bash
i/I/a/A/o/O		# switch insert mode
r/R			# replace
v/V			# switch view mode
e/b			# move the word of end/begin
''			# 光标回到上一次的位置
%			# 跳到括号匹配处

z			# 将当前行移动到屏幕的中间
zt 			# 将当前行移到屏幕顶部
zb 			# 将当前行移到屏幕底部
ZZ			# 保存并退出
gf			# 打开头文件
gt			# goto next tab
gT			# goto prev tab
[[ 			# 跳转至上一个函数
]] 			# 跳转至下一个函数
x			# delete char
dw			# delete word
dd			# delete line
D			# 删除光标之后的字符串
d0			# 删除光标之前的字符串
~			# 大小写转换
u			# 撤销


<C-e> / <C-y>		# 屏幕向下(上)移动一行
<C-w-w>			# switch window
<C-w-r>			# swap window

<C-a>			# add 1
<C-x>			# sub 1
<C-r>			# 撤销回退
<C-n>			# 代码补全
<C-w-s>     		# 水平新建窗口
<C-w-v>     		# 垂直新建窗口
<C-g>			# 显示文件信息
<C-o>			# 回到上次打开的文件
```

# insert mode
```bash
:e file2		# 当前窗口打开
:sp file2 		# 水平切分窗口
:vsp file2 		# 垂直切分窗口

:ls			# list open files
:bn			# switch file-n

# tabe相关
:tabe file...		# new table file...

:verbose set mouse	# 查看最后在哪里配置的mouse
:w ! sudo tee %		# 强制保存

:vert diffsplit file2	# 比较两个文件不同

:5,15s/dog/cat/g	# 替换行内字符串
:%s/dog/cat/g
```

## 设置空格或回车可见
```bash
:setlocal list
:set listchars=tab:>~,trail:.
```

# others
## 折叠相关
```bash
z f %			# 折叠
z o			# 展开
z c			# 折叠
```


## plugins
### vim-markdown
```bash
zr: reduces fold level throughout the buffer
zR: opens all folds
zm: increases fold level throughout the buffer
zM: folds everything all the way
za: open a fold your cursor is on
zA: open a fold your cursor is on recursively
zc: close a fold your cursor is on
zC: close a fold your cursor is on recursively
```
