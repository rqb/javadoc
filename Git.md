### Git工作原理

> - Workspace：工作区
> - Index / Stage：暂存区
> - Repository：仓库区（或本地仓库）
> - Remote：远程仓库

````sequence
Remote->Repository:fetch/clone
Repository->Remote:push
Index->Repository:commit
Repository -> WorkSpace:checkout
WorkSpace->Index:add
Remote->WorkSpace:pull
````



### 创建新的仓库

…or create a new repository on the command line

```
echo "# test" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/rqb/test.git
git push -u origin master
```

### 提交存在的仓库

…or push an existing repository from the command line

```
git remote add origin https://github.com/rqb/test.git
git push -u origin master
```

### 从另外一个仓库导入

…or import code from another repository

You can initialize this repository with code from a Subversion, Mercurial, or TFS project.



###提交你的修改

1、先用 git status 看你更改了哪些文件；

2、然后 git add 你想要提交的更改的文件 或者 git add . 所有的文件；

3、再git commit -m ‘提交信息’；

4、最后 git push origin master/你的分支 。



### 删除远程仓库中的文件（夹） 

git rm -r --cached a/2.txt                    		// 删除a目录下的2.txt文件 

git rm -r --cached  HSYDEV-1935/   		 //删除文件夹

git commit -m  "删除a目录下的2.txt文件"  // commit

git push



### 从github上克隆

 git clone https://github.com/rqb/javadoc



### Git操作之克隆某一个特定的远程分支

git clone -b <branch name> [remote repository address] 













