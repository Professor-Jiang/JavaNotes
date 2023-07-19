# Git命令

`git fetch:`  git fetch会将数据拉取到本地仓库，它并不会自动合并或者修改当前的工作。

`git pull: `git pull是从远程获取最新版本并merge到本地，会自动合并或修改当前的工作。

`git branch -a:` 查看本地和远程的所有分支。

![image-20230719095118755](/Users/jiang/Desktop/Workspace/笔记文档/Git命令.assets/image-20230719095118755.png)

`git branch -r:` 查看远程所有分支。

![image-20230719095125287](/Users/jiang/Desktop/Workspace/笔记文档/Git命令.assets/image-20230719095125287.png)

`git branch <branchname>:` 新建分支

`git branch -d <branchname>:` 删除本地分支

`git branch -d -r <branchname>：` 删除远程分支，删除后还须推送到服务器

`git branch -m <oldbranch> <newbranch>:` 重命名本地分支

## 如果只是在本地创建了一个分支，远程git仓没有，那么git push时会报错:

![image-20230719095706328](/Users/jiang/Desktop/Workspace/笔记文档/Git命令.assets/image-20230719095706328.png)

需要使用` git push --set-upstream origin <branchname>`命令。

![image-20230719095749477](/Users/jiang/Desktop/Workspace/笔记文档/Git命令.assets/image-20230719095749477.png)

## 如何删除远程分支？

![image-20230719100003499](/Users/jiang/Desktop/Workspace/笔记文档/Git命令.assets/image-20230719100003499.png)

对于之前删除过的分支，我们可以使用git push直接推送从而复建分支。

![image-20230719100132704](/Users/jiang/Desktop/Workspace/笔记文档/Git命令.assets/image-20230719100132704.png)

## 如何重命名本地分支和远程分支?

![image-20230719100314146](/Users/jiang/Desktop/Workspace/笔记文档/Git命令.assets/image-20230719100314146.png)

**重命名本地分支没问题，远程呢？**

```java
/**
*重命名远程分支：
*1、删除远程待修改分支
*2、push本地新分支到远程服务器
*/
```