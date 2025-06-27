# Git Pull and Merge


<!--more-->
# pull 新代码与合并冲突

## 问题
通常我在pull新代码时的操作是：
1. 切换到main分支，pullmain分支代码
2. 切换到自己的分支 
3. git rebase main 通过变基操作合并代码。 

但是**经常产生冲突！！！导致写完的代码消失不见**

## 解决
梁老师通常直接在自己的分支上 `git pull origin main` ，pull命令就包含了fetch和merge，如果没有冲突的话会自动合并，有冲突的话会提示解决，但这种操作产生的冲突通常并不多

## 注意
1. 最好每次敲完代码都commit上去，就算没MR，提交到服务器上也不会导致本地代码消失
2. 如果真的存在很多冲突且最新的commit已经push的话，完全可以重新clone项目，下载自己的分支，如gpgsm分支：'git branch gpgsm origin/gpgsm'，再pull下来main分支`git pull origin main`，这样也会减少很多麻烦


---

> 作者: tearizz  
> URL: http://localhost:1313/posts/0663fe4/  

