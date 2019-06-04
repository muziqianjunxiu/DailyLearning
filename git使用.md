# git使用

## SSH创建

由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容，点“Add Key”，你就应该看到已经添加的Key。当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

## 本地链接远程库

`git   init`		初始化一个仓库

`git  remote add origin  git@github.com:muziqianjunxiu/DailyLearning.git`		添加远程库

`git  pull  origin  master`		将远程库内容拉入刚创建的本地库

`git push  -u  origin  master`			推送到远程库

`git push  -f  origin  master`	  强制推送

`git  clone  git@github.com:muziqianjunxiu/DailyLearning.git`		从远程库克隆

`git checkout -b dev`			创建分支dev并切换到dev

`git branch`		查看当前分支

`git  checkout  master`		切换回master分支

`git  merge  dev`		将dev分支合并到master分支上（快速合并）

`git merge --no-ff -m "merge with no-ff" dev`		合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

`git  branch  -d  dev` 		删除dev分支

 `git log --graph --pretty=oneline --abbrev-commit`		查看分支的合并情况

`git   log` 			查看分支合并情况

