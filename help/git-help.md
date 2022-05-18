git format-patch -1  
./scripts/checkpatch.pl 0001-loongarch-support-pagesize-12-and-14-bits.patch  

git add -u  
git commit --amend  
git commit -s --amend
git push origin HEAD:refs/for/uos-dev-3.1.0  


# 文件回退到某个版本
git checkout e85be088dd4023edaa target/loongarch/helper.c 
# patch有冲突时
git apply –reject xxx.patch
