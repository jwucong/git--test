# git--test

1. **cherry-pick** 
   > 精心挑选某分支的某次commit合并到某分支，例如想吧dev的某次新开发的功能直接合并到master
   ```shell
   git checkout master
   git cherry-pick [commitId]
   git status  # 如果有冲突则需要手动解决冲突
   git push
   ```
   
2. **revert**
   > 保持历史所有的commit并用一次新的commit回到某个历史commit，常用于远程分支的回退
   ```shell
   git checkout dev
   # 此处commitId是你需要回退到的那次commit的后一次commitId
   # 如：A -> B -> C -> HEAD,你需要回退到B，则此处commitId为C的commitId
   git revert [commitId] 
   git status  # 如果有冲突则需要手动解决冲突
   git push
   ```
   
3. **reset**
   > 版本从HEAD回退到某个历史的commit A, 并将从A到HEAD之间的历史commit删除，常用于本地分支回退  
     --soft  保持index，即保持已经git add后的修改  
     --mixed 撤销index到working tree，即保持本地修改，但需要从新git add  
     --hard  删除working tree中的修改，直接恢复一个干净的工作区  
     --merge 撤销index到working tree并合并两次提交，需要从新git add
   ```shell
   git checkout dev
   # 此处commitId为你想要回退到的那次commitId
   git reset --[soft|mixed|hard|merge] [commitId] 
   git status  # 如果有冲突则需要手动解决冲突
   git push
   ```

add 1
add2
