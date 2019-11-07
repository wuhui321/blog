随着团队中开发工程师数量增加，由原来一个工程师负责一个前端项目逐步转变成由多人同时开发一个项目。最近在项目开发过程中发现在项目版本管理的Git仓储中对分支管理以及分支Commit信息存在些许不规范情况，故此跟大家分享下Git中的一些原理和使用技巧。

一、聊下GIt仓储的结构，包括：本地工作区、暂存区、本地仓库、远程仓库副本以及远程仓库。

例如，从我们每天早晨到工位到晚上下班从公司出发回家来举例说明：

1、首先到单位的第一件事儿是

git checkout feature

// 本地项目切换到feature分支，此操作其实是将HEAD指针指向feature分支

2、从远端拉取最新Code

git pull origin feature

// 此pull 操作本质上是个复合操作，包含执行：git fetch + git merge origin feature 两个操作，会自动从远端仓库拉取Code同时copy在远程仓库中再和本地代码进行和合并

3、每天开发前的准备工作完毕后，开始根据排期进行项目开发，项目开发完毕后，开始提交Code

git add

// 此操作是将本地工作区的Code添加到暂存区（index stage）

git commit

// 此操作是将暂存区的代码添加到本地仓库（local repository）

git push

// 此操作是将本地仓库中的代码推送到远程仓库（包括：远程仓库、远程仓库副本）

4、满心欢喜下班回家

上述流程总结成流程图，如下：


![](https://user-gold-cdn.xitu.io/2019/11/6/16e3f773a9e946d5?w=970&h=408&f=png&s=27390)
二、git pull 与 git fetch 的区别

由第一部分的“Git流程图”中我们可以发现，git pull 指令其实执行的是两个指令，通常我们说这种指令是复合指令，包含的指令如下：

1、git fetch

2、git merge

其中，git fetch 指令是将远程仓储中的Code 推送到远程仓库副本的地方；git merge origin feature是将远程仓库副本的代码与本地仓库中代码进行合并。在平时开发中，如果你的项目是多人协作的，尽量少用pull，而是采用fetch + merge的方式。

三、git merge、git merge --no-ff 与 git rebase 的区别

当我们的feature分支代码开发完，QA测试完毕后需要和主干代码进行合并，如果你的项目是一人独立负责可以忽略此部分。但是在很多情况下一个项目是由多人共同维护，此时相当于master 主干分支和你自己维护的feature分支都在同时前进，此时在合并代码时或多或少存在代码冲突；不仅如此，feature分支的提交历史记录和master主干分支团队其他人提交的记录都会不仅是一条，在此情况下得代码合并，很多人都是直接执行如下代码：

1、git checkout master

2、git merge feature

3、git push

此种合并分支代码的方式在项目维护人员不多或仅自己在维护时比较适用；但是在多人员维护代码时不适用但是可以依旧以此应用，只是会对master主干分支的历史提交记录混乱，在后期master主干分支出现问题需要回滚代码时带来些许代价。因此，有些多人维护项目的团队中会约定使用git rebase 进行代码合并。那么，接下来和大家分享下git merge 、git merge --no-ff 与 git rebase的区别。


![](https://user-gold-cdn.xitu.io/2019/11/6/16e3f79395e502d8?w=1390&h=1198&f=png&s=260310)


git merge、git merge --no-ff 与 git rebase的区别
四、git reset、git revert 与git checkout 之间的区别

前面我们梳理了

git pull与git fetch的区别

git merge 、git merge --no-ff与git rebase的区别

相信大家已经非常清晰了能够以最优方式在自己团队中使用Git常用的代码拉取、代码合并的指令，但是天有不测风云，大家虽不期望但总会出现一些重大问题需要对代码进行回滚。那么，和大家分享下代码回滚的git reset、git revert以及git checkout 之前的区别。

git reset 是将当前分支的HEAD指向历史的某个特定提交，空余出的提交记录会在git下次执行垃圾回收的时候删除掉。

git revert 其实是将想要回滚的版本代码当做一次新的修改代码进行提交，以前的间隔版本依然存在

git checkout 在提交层面上是用来切换分支，在文件层面是来舍弃工作区的更改

总结，如下：


![](https://user-gold-cdn.xitu.io/2019/11/6/16e3f7954f284b25?w=1038&h=540&f=png&s=108654)


详细大家在通过上面的描述已经对Git常用的拉取代码、合并代码、回滚代码有一定理解，有问题大家可以在评论区进行讨论。

最后，和大家分享下我比较喜欢的Git Tip网站：
[https://github.com/k88hudson/git-flight-rules]()
