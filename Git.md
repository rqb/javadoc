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



### 从github上克隆

 git clone https://github.com/rqb/javadoc



### Git常用命令

> - Workspace：工作区
> - Index / Stage：暂存区
> - Repository：仓库区（或本地仓库）
> - Remote：远程仓库

```sequence
Remote->Repository:fetch/clone
Repository->Remote:push
Index->Repository:commit
Repository->WorkSpace:checkout
WorkSpace->Index:add
Remote->WorkSpace:pull
```





