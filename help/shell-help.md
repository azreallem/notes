# 常用命令
```bash
# 2>&1 其中2表示错误输出，输入到标准输出1中。
ls readme.txt 1.txt >out.log 2>&1
ls -alt ./

# 复制文件
cp -a /home/gaoliang/work/linux-loongson/  gaoliang/

# 查找文件
find ./ -name "main.c"

# 比较两个文件
vim -d file1 file2
vim diff file1 file2

# grep匹配字符串
grep -nr "..." ./...
grep -nr "..." ./... | grep -v ".o"
grep -nr "CSR_TLBRERA" --exclude=*.o
grep -Iinr "str" ./

# 替换目录下文件中的字符串
sed -i "s/lcsrintrin/loongisa_csr/g" `grep 'lcsrintrin.h' -rl ./arch/loongarch`

# 打包文件
tar -cvf filename.tar.gz dirname
# 解压文件
tar -xvf filename.tar.gz
## -c, --create
## -x, --extract

# 索引
ctags -R

# kill
ps -aux | grep gaoliang
pkill -u gaoliang
killall -u gaoliang

# 从第3000行开始，显示1000行。即显示3000~3999行
cat filename | tail -n +3000 | head -n 1000
```

