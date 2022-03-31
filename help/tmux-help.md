## session操作
```bash
tmux new　　			# 创建默认名称的会话
tmux new -s mysession		# 创建名为mysession的会话
tmux ls　　			# 显示会话列表
tmux a				# 连接上一个会话
tmux a -t mysession　		# 连接指定会话,tmux a -t 1
tmux rename -t s1 s2		# 重命名会话s1为s2
tmux kill-session		# 关闭上次打开的会话
tmux kill-session -t s1		# 关闭会话s1
tmux kill-session -a -t s1	# 关闭除s1外的所有会话
tmux kill-server		# 关闭所有会话
```

## window操作
```
prefix c	# 创建一个新窗口
prefix ,	# 重命名当前窗口
prefix n	# 进入下一个窗口
prefix p	# 进入上一个窗口
prefix l	# 进入之前操作的窗口
prefix 0~9	# 选择编号0~9对应的窗口
prefix .	# 修改当前窗口索引编号
prefix '	# 切换至指定编号（可大于9）的窗口
prefix f	# 根据显示的内容搜索窗格
prefix s	# 列出窗口
ctrl + d	# 关闭当前窗口
```

## 窗格管理
```
prefix %			# 水平方向创建窗格
prefix "			# 垂直方向创建窗格
prefix Up|Down|Left|Right	# 根据箭头方向切换窗格
prefix q			# 显示窗格编号
prefix o			# 顺时针切换窗格
prefix }			# 与下一个窗格交换位置
prefix {			# 与上一个窗格交换位置
prefix x			# 关闭当前窗格
prefix space			# 重新排列当前窗口下的所有窗格
prefix !　　			# 将当前窗格置于新窗口
prefix Ctrl+o　　		# 逆时针旋转当前窗口的窗格
prefix t　　			# 在当前窗格显示时间
prefix z　　			# 放大当前窗格(再次按下将还原)
prefix i　　			# 显示当前窗格信息
```

## down-cmd
:move-pane -t 0:1	# 移动窗格到session0:1

## 常用方法
- `prefix %`：划分左右两个窗格。
- `prefix "`：划分上下两个窗格。
- `prefix ->`：光标切换到其他窗格。
- `prefix d`：隐藏tmux
- `prefix space`：重新排列当前窗口下的所有窗格`

```bash
tmux ls	
tmux a -t "name"
tmux kill-session "name"
```

```
prefix x	# 杀死窗格
```
