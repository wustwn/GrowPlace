## GIT

1. ### reset & checkout one file from HEAD commit
```
   1. git reset HEAD^ -- file_to_checkout_A
   2. git commit --amend --no-edit
```   
   注意：执行完1命令时对文件A的修改撤回操作已经在暂存区，这时如果需要继续修改，可以继续进行修改再进行add,commit --amend，或者先执行2，再进行修改add, commit --amend.
   now the HEAD commit does not include file_A's changes

2. ### Add multiple remote:
```
git remote add DIY_NAME GIT_REPO_PATH
```

3. ### Show remote details information:
```
git remote show NAME
```

4. ### Git reset single file from commit
如果想将某文件reset到与当前HEAD commit之前的状态，可以先使用diff查看具体的修改内容，然后再使用reset。需要注意的是，commit-id 是超前的一条commit。
```git
git diff commit-id /path/to/file
git reset commit-id /path/to/file
```
5. ### Git clone 单个分支 [save repo size]
```git
git clone -b  branch_name --single-branch repo_path
```   
6. ### Personal access token (PAT)
自2021.8.13后,github不再使用`username` & `pwd`,使用`PAT`进行替代,即需要对应生成`PAT` 来替代`password`, 原来使用可以使用
```git 
git clone https://username:pwd@repopath 
```
现在需要替换为:
```git
git clone https://username:token@repopath
```
