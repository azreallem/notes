git format-patch -1
./scripts/checkpatch.pl 0001-loongarch-support-pagesize-12-and-14-bits.patch

git add -u
git commit --amend
git push origin HEAD:refs/for/uos-dev-3.1.0
