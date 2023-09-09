# 一、环境设置

假设有两个终端，正在开发同一项目，右侧是我，左侧是小明在开发。此时二人都是刚克隆的主分支master。

![image-20230803213358844](./git学习.assets/image-20230803213358844.png)

## 1.1开发不同文件

接下来，二者开始开发自己的项目。小明开发完毕后，首先提交。（注意开发必须新建自己的分支）

![image-20230803214022482](/Users/jiang/Desktop/Workspace/笔记文档/git学习.assets/image-20230803214022482.png)

如下图，此时遇到错误：

```java
当前分支 xiaoming 没有对应的上游分支。

为推送当前分支并建立与远程上游的跟踪，使用

  git push --set-upstream origin xiaoming
```

![image-20230803214148139](./git学习.assets/image-20230803214148139.png)

此时，按照命令执行即可。命令作用就是在远程origin建立xiaoming分支，然后push。记得push前应该先pull远程主分支，然后push自己的分支，然后在远程上操作合并。

![image-20230803214419077](./git学习.assets/image-20230803214419077.png)

![image-20230803215154099](./git学习.assets/image-20230803215154099.png)

一般是提交申请，然后有人会在主分支上将小明提交的分支合并。

![image-20230803215550754](./git学习.assets/image-20230803215550754.png)

选择，`审查通过，测试通过，合并分支`代码即可合并。

![image-20230803215635745](./git学习.assets/image-20230803215635745.png)

此时看下自己的gitee，两个分支都在，且master分支合并了xiaoming分支。下面分别是合并前和合并后的，注意注释a.txt的commit内容已经和分支的一样了。

**合并前：**

![image-20230803215908714](./git学习.assets/image-20230803215908714.png)

**合并后：**

![image-20230803220208173](./git学习.assets/image-20230803220208173.png)

![image-20230803220230643](./git学习.assets/image-20230803220230643.png)

jiang此时正在开发代码（代码落后于主分支），jiang开发完成以后，因为不知道master已经被几个人改变过了，所以写完代码后，首先应该pull远程的master，然后push。这里我们使用下idea git操作。

我在这里先看看都有哪些分支，直接点击蓝色的`Fetch箭头`即可，然后开始pull远程主分支。

![image-20230803220538661](/Users/jiang/Desktop/Workspace/笔记文档/git学习.assets/image-20230803220538661.png)

![image-20230803220707412](./git学习.assets/image-20230803220707412.png)

然后我们会遇到以下报错：

![image-20230803220734999](./git学习.assets/image-20230803220734999.png)

具体内容：

```java
POST git-upload-pack (317 bytes) From https://gitee.com/Fichtner/test-pull * branch master -> FETCH_HEAD = [up to date] master -> origin/master hint: You have divergent branches and need to specify how to reconcile them. hint: You can do so by running one of the following commands sometime before hint: your next pull: hint: hint: git config pull.rebase false # merge hint: git config pull.rebase true # rebase hint: git config pull.ff only # fast-forward only hint: hint: You can replace "git config" with "git config --global" to set a default hint: preference for all repositories. You can also pass --rebase, --no-rebase, hint: or --ff-only on the command line to override the configured default per hint: invocation. Need to specify how to reconcile divergent branches.
```

大概就是说你的当前文件的master版本已经落后于主版本了，需要你指定如何pull master。其实细心一点的话会发现在pull的时候是有选项的。

![image-20230803221048608](./git学习.assets/image-20230803221048608.png)

这里我们使用no-commit，将代码合并，但是先不提交。

![image-20230803221211955](./git学习.assets/image-20230803221211955.png)

```java
POST git-upload-pack (317 bytes) From https://gitee.com/Fichtner/test-pull * branch master -> FETCH_HEAD = [up to date] master -> origin/master hint: You have divergent branches and need to specify how to reconcile them. hint: You can do so by running one of the following commands sometime before hint: your next pull: hint: hint: git config pull.rebase false # merge hint: git config pull.rebase true # rebase hint: git config pull.ff only # fast-forward only hint: hint: You can replace "git config" with "git config --global" to set a default hint: preference for all repositories. You can also pass --rebase, --no-rebase, hint: or --ff-only on the command line to override the configured default per hint: invocation. Need to specify how to reconcile divergent branches.
```

此时，你会发现仍然报之前的错误。说明此路行不通。再来试一下rebase行不行。

![image-20230803221452467](./git学习.assets/image-20230803221452467.png)

![image-20230803221519237](./git学习.assets/image-20230803221519237.png)

此时看一下，成功了。所有内容都是对的。

![image-20230803221619309](/Users/jiang/Desktop/Workspace/笔记文档/git学习.assets/image-20230803221619309.png)

## 1.2开发相同文件

那么再来试一下xiaoming和jiang开发相同的文件，假设是`README.md`。

![image-20230803222030931](./git学习.assets/image-20230803222030931.png)

编辑完毕后，先自己提交，然后pull matser，然后提交，然后提交合并申请，远程同意合并。

![image-20230803222209783](./git学习.assets/image-20230803222209783.png)

![image-20230803222250182](./git学习.assets/image-20230803222250182.png)

此时又有一个问题，push警告，这里主要是因为我们之前在远程合并了分支，IDEA检查到这个分支的版本不一致造成的【猜测【被合并方】合并分支会造成新版本，有点奇怪，我只有在第一次合并完，修改文件push的时候遇到了这个警告，之后再没遇见】，不知道应该如何push，选merge即可。

![image-20230803224628013](./git学习.assets/image-20230803224628013.png)

可以看到README.md文件已经修改被修改了。此时按照之前说的，提请求，然后主分支同意合并。

![image-20230803225519462](./git学习.assets/image-20230803225519462.png)

假如这里不填写commit信息，会造成新版本吗？必须填写合并信息，不然流程进行不了。

![image-20230803225717497](./git学习.assets/image-20230803225717497.png)

可以看到此时主分支已经合并成功了。

![image-20230803225858467](./git学习.assets/image-20230803225858467.png)

现在jiang在相同的文件修改内容，然后先push自己的分支，然后pull master，然后再push，然后再提交合并请求。

![image-20230803230712318](./git学习.assets/image-20230803230712318.png)

![image-20230803230933758](./git学习.assets/image-20230803230933758.png)

![image-20230803231003765](./git学习.assets/image-20230803231003765.png)

将远程拉取到当前dev-jiang分支。

![image-20230803231034941](./git学习.assets/image-20230803231034941.png)

如我们所料，报错了，因为jiang的主分支版本已经落后了。

![image-20230803231123415](./git学习.assets/image-20230803231123415.png)

使用rebase解决。

![image-20230803231313640](./git学习.assets/image-20230803231313640.png)

此时，可以看到出现了合并冲突，询问你如何处理，是合并还是二选一。

![image-20230803231437845](./git学习.assets/image-20230803231437845.png)

当出现这种情况时候，我们需要merge。另外说一下，左下角打勾是“显示文件夹“的作用。

![image-20230803231631790](./git学习.assets/image-20230803231631790.png)

选择merge之后，出现下面的三列，左边是jiang的，右边是master的，中间是最终结果。

![image-20230803231739979](./git学习.assets/image-20230803231739979.png)

自己选择即可。

![image-20230803232006078](./git学习.assets/image-20230803232006078.png)

如上图右上角，所有冲突解决了。点击apply即可。

现在如果我们已经不落后于主版本了，直接push即可。如下图，可以看到之前落后于主版本，主版本都有哪些更新日志。

![image-20230803232238200](./git学习.assets/image-20230803232238200.png)

此时又遇到了之前的警告。我们仍然选merge即可。

![image-20230803232347535](./git学习.assets/image-20230803232347535.png)

可以看到又出现了多版本的代码，这是因为我们本地的dev-jiang刚合并了主分支，相比于之前的dev-jiang有了变动。可以再初一遍冲突或者在这里直接选择accept yours。【如果在点击Accept yours后告警没推送成功，那么再push下就成功了】

![image-20230803232447890](./git学习.assets/image-20230803232721671.png)

可以看到远程dev-jiang上已经全部正确了。

![image-20230803232836687](./git学习.assets/image-20230803232836687.png)

然后正常提交请求，合并到主分支即可。主分支合并完成。

![image-20230803233059637](./git学习.assets/image-20230803233059637.png)