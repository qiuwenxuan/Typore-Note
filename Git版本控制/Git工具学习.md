## git常用命令



```shell
git clone <url> # 克隆远程仓库

git add <...>
	git add filename # 将本地文件添加到git暂存区
	git add . # 添加修改的文件到暂存区
	git add --all /-A # 添加所有的文件到暂存区
	
git commit -m "message" # 将暂存区更改提交到本地仓库

git push # 推送本地仓库到远端仓库
	git push -u # 将本地分支推送到远端同名分支
	git push -f # 强制推送
	git push --all # 将本地所有分支的修改都统一提交远程仓库
	git push origin master # 推送到远程仓库master分支
	
git pull # 拉取远端代码到本地仓库
	git pull origin <branch-name> # 拉取指定远端仓库到本地
	
git status # 查看当前工作目录和暂存区变更的文件名

git remote -v # 查看远程仓库的ssh

git diff # 查看修改的文件内容

git log # 查看提交的日志信息和序列号，序列号可以用来回滚历史

git reset --hard <commit-id> # 回滚到指定历史版本
git reset HEAD^ # 撤销最近一次提交

git branch -a # 列出所有的远程分支
	git branch -u origin/<branch-name> <branch-name> # 将本地分支与远端分支关联

git checkout <branch-name> # 切换到指定分支

git remote add origin <ssh-url> # 新建本地仓库时首次关联远程仓库和指定远程仓库的URL
git remote set-url origin <ssh-url> # 更改远程仓库的URL

git fetch # 从远程仓库中获取（拉取）最新的提交和数据，但不会自动合并到您当前的工作分支中
git merge <branch-name> # 将一个分支的修改合并到另一个分支上,会在当前分支创建一个新的提交
git rebase <branch-name> # 将当前分支移动到目标分支上，通常用于修改并清理提交历史，可能会导致冲突
```

## git免密操作

```shell
配置个人信息：
git config --global user.name "Wenxuan Qiu"     #首字母大写
git config --global user.email "wenuxan.qiu@jaguarmicro.com"

生成密钥：
ssh-keygen -t rsa -C "wenuxan.qiu@jaguarmicro.com"
或者：
ssh-keygen -t ed25519 -C "wenuxan.qiu@jaguarmicro.com"

查看完整密钥：
cat ~/.ssh/id_rsa.pub

将密钥添加到代码托管平台的ssh key就完成免密操作啦
```



## 场景一、git pull最新的代码，但是本地有暂存区的修改

![image-20240205105440343](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240205105441.png)

![image-20240205105423697](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240205105427.png)

这个错误发生在使用 `git pull` 命令时，Git 检测到你的工作目录中有未提交的更改，这些更改与即将从远程仓库合并的更改存在冲突。Git 为了保护你的更改不被覆盖，要求你先解决这些未提交的更改。

1.提交更改：如果你想保留这些更改并继续合并，你可以提交这些更改。这样，你就可以使用 `git pull` 命令而不会丢失任何工作。

```
git add .
git commit -m "提交信息"
```

2.**暂存更改（Stash）**：：如果你不想立即提交你的更改，可以使用 `git stash` 命令将更改暂存起来。暂存操作会保存你的工作目录和索引的状态，并让工作目录干净。

```
git stash push -m "暂存信息"
git pull
git stash pop 
```

3.**放弃更改**：如果你决定放弃工作目录中的更改，可以使用 `git checkout` 或 `git restore`（取决于你的 Git 版本）命令来撤销这些更改。

```
git restore --source=HEAD --staged .
git restore .
```

## 场景二、本地仓库已有代码，需要关联提交到空的远程仓库

```
git init
git add --all
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:qiuwenxuan/sw_itest.git
git push -u origin main
```



## git merge 和 git rebase的区别

使用场景：我们有一个master分支，在master分支的commit1上创建一个itest分支，现在master分支更新了commit2,y原本的itest分支需要更新master分支，应该怎么做

```
      commit 1   commit 2
         |         |
         * ---- * ---- * (master)
                  \
                    * ---- * (itest)

```

### 1.使用git merge

在itets分支上，执行 `git merge master` ，将在 `itest` 分支上创建一个新的合并节点：

```
      commit 1   commit 2
         |         |
         * ---- * ---- * --------- * (master)
                  \               /
                    * ---- * ---- * (itest)

```

### 2.使用git rebase

相当于在原本的commit1上创建的itest分支不变，itest分支平滑的移动到commit2

```
      commit 1   commit 2
         |         |
         * ---- * ---- * (master)
  平滑的移动到commit2--> \
                          * ---- * (itest)

```

类似于这种：

```
        /o-----o---o--o-----o--------- branch
--o-o--A--o---o---o---o----o--o-o-o--- master
```

```
                                   /o-----o---o--o-----o------ branch
--o-o--A--o---o---o---o----o--o-o-o master
```



## **合并冲突**

合并冲突是指你在不同的分支上提交对同一行的更改，产生了冲突。如果发生这种情况，Git 将不知道要保留哪个版本的文件，并显示类似于以下内容的错误消息：

```shell
CONFLICT (content): Merge conflict in resumé.txt Automatic merge failed; fix conflicts and then commit the result.
```

当出现冲突的时候你选择自动合并冲突，系统会自动在冲突的地方做上一下标记，此时需要我们手动的更改这些冲突然后使用add .提交

1. **合并冲突标记：** Git 会在冲突的文件中插入特殊的标记，通常是一些注释，用于标识冲突的区域。这些标记包括：

   - `<<<<<<<`：冲突的起始标记，下面的内容是当前分支（本地分支）的修改。
   - `=======`：分隔符，下面的内容是要合并的另一分支的修改。
   - `>>>>>>>`：冲突的结束标记。

   一个例子可能如下所示：

   ```
   plaintextCopy code<<<<<<< HEAD
   // 本地分支的修改
   =======
   // 要合并的另一分支的修改
   >>>>>>> branch-name
   ```

2. **手动解决冲突：** 自动合并后，你需要手动打开包含冲突的文件，查看并解决冲突。你需要选择保留哪一部分或者进行修改，以解决冲突。在解决冲突后，你需要再次提交文件，这次提交将包含你的解决方案。

3. **提交合并结果：** 解决冲突后，使用`git add`将文件标记为已解决，然后使用`git commit`提交合并结果。这个提交将包含你的解决方案并完成合并过程。

总的来说，自动合并并不总是能够成功解决冲突，尤其是在两个分支对同一部分进行了相似但不同的修改时。在这种情况下，手动解决冲突是必须的。





# 一、Git版本操作

## 1、Git工作流程

![image-20231106141145005](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106141145005.png)

## 2、本地仓库操作

### 1.本地仓库的创建

Git工作流程都是围绕着本地仓库管理的，因此首先我们需要创建一个==本地仓库Repository==

![image-20231106141726358](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106141726358.png)

创建完成之后会在项目文件夹内生成一个.git的文件夹，本地仓库的文件夹被称为==工作区workspace==

![image-20231106141805433](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106141805433.png)

### 2.将工作区的文件添加在暂存区

右键需要添加的文件，选择添加

![image-20231106142225071](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106142225071.png)

添加后的文件会显示绿色勾号，表示文件已经添加在==暂存区==

![image-20231106142228036](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106142228036.png)

### 3.将暂存区文件提交到本地仓库

将放在暂存区的文件右键提交到本地仓库

![image-20231106142708469](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106142708469.png)

### 4.修改文件并提交到本地仓库

将文件在工作区内修改之后，我们直接右键将工作区文件提交到本地仓库即可，无需先提交到暂存区（git会自动给我们提交到暂存区）

![image-20231106143022982](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106143022982.png)

当让我们还可以双击参看我们修改的文件与上一版本的不同

![image-20231106143326867](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106143326867.png)

### 5.查看日志（版本记录）

![image-20231106143609968](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106143609968.png)

### 6.还原

如过你再工作区进行了很多无效的修改操作，你想将其恢复到与本地仓库最后一次提交一致的内容，点击还原即可

工作原理就是git会将本地仓库的最后一次提交的内容覆盖到你的工作区的内容

![image-20231106143806068](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106143806068.png)

### 7.删除文件

先在工作区内删除文件夹，后点击提交到本地仓库，本地仓库的文件才会跟着删除，否者只是工作区删除了，本地仓库没有删除文件夹。

我们可以点击还原还原上一步删除的文件，也可以点击日志找到已删除的文件版本并重新导出当时的版本

### 8.忽略文件

我们在实际的开发项目中，会在工作区产生一些不需要提交在本地仓库的文件，我们可以将这个或者这类文件去选择性的去忽略，这样当我们提交其他文件时，就不会将此类文件也提交到本地仓库

举个例子：在java开发中.java文件在编译后会产生同名的.class文件，我们不希望在本地仓库中添加这个.class文件，因此我们一般提交的时候会选择忽略这类.class文件

这里我们可以选择忽略这个文件User.class，也可以选择忽略以.class后缀名的所有文件；将其添加在忽略列表当中

![image-20231106150203216](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106150203216.png)

我们可以选择只忽略指定文件夹内的文件或文件夹，还是所有文件夹内递归忽略文件

![image-20231106151224407](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106151224407.png)

git会将忽略的文件类型添加在.gitignore文件当中，我们也可以修改这个文件配置需要忽略的文件类型

当选择只在指定文件夹内忽略文件类型时，忽略配置文件.gitignore会显示`*/*.xxx`表示只在指定根文件内忽略，当选择递归忽略该文件类型时，会显示`*.xxx`表示所有文件夹都将递归忽略该文件类型

![image-20231106150632961](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106150632961.png)

## 3、ideal使用git版本控制

我们在原项目文件夹内创建Git仓库

![image-20231106151938852](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106151938852.png)

创建完之后可以发现VCS已经变成Git字样

![image-20231106152054409](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106152054409.png)

这个时候，旁边也多了一个commit提交的按钮，我们可以使用这个菜单对代码和文件进行版本控制

一般我们不会把idea配置文件上传到本地仓库，因此我们可以把它添加到.gitignore文件进行一个忽略

![image-20231106152421800](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106152421800.png)

## 4、远程仓库操作

![image-20231106155218092](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106155218092.png)

当你在本地仓库创建的文件需要被另一个人共享时，此时需要将本地仓库提交到远程仓库，他人可以直接通过互联网访问到你的远程仓库并对你的远程仓库做一些代码的提交

### 1.创建远程仓库

这里以gitee(码云)为例，我们可以去gitee官网创建一个远程仓库，创建完可以得到远程仓库的地址，有如下两种格式：

- HTTPS
- SSH

![image-20231106153716650](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106153716650.png)

#### 1.1 采用https地址登录

采用Https地址，当我们访问远程仓库时，需要登录用户名和密码，而且win10以上系统会自动记住你的账号和密码，下次登录账号无需再进行验证

#### 1.2 采用SSH登录本地仓库

由于每次同步代码都需要输入密码既麻烦又不安全，建议在私人电脑使用SSH公钥方式同步代码。使用SSH公钥可以让你在你的电脑和Gitee通讯的时候使用安全连接。因此采用SSH秘钥登录

创建公钥

在本地工作区打开git控制台，输入一下代码生成git公钥

![image-20231106170759375](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106170759375.png)

生成公钥命令行，系统将公钥存储在user/ssh文件夹下的id_rsa.pub文件当中

```
 ssh-keygen -t rsa -C "wenxuan.qiu@jaguarmicro.com"
```

![image-20231106170739149](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106170739149.png)

查看公钥

```
cat ~/.ssh/id_rsa.pub 
```

![image-20231106172351341](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106172351341.png)

添加公钥

![image-20231106172521852](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106172521852.png)

添加公钥后，此时再使用ssh地址克隆代码，就不需要登录验证了，相当于免密登录

### 2.克隆远程仓库

我们在得到远程仓库的地址后，需在本地创建一个本地仓库，并将远程仓库的代码通过仓库地址克隆到本地仓库，再对本地仓库的代码做一些修改后提交到远程仓库

![image-20231106154114870](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106154114870.png)

![image-20231106154150366](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106154150366.png)

### 3.代码推送

我们使用推送功能将本地仓库的代码推送到远程仓库

![image-20231106160219653](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106160219653.png)

![image-20231106160154672](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106160154672.png)

### 4.代码拉取

当我们将远程仓库的代码克隆到本地仓库，倘若远程仓库修改了最新版本，我们可以通过拉取功能拉取到远程仓库的最新版本。

![image-20231106160722758](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106160722758.png)

### 5.本地仓库关联远程仓库

将本地仓库的版本推送到远程仓库（即先有本地仓库，再创建一个远程仓库将本地仓库上的代码和版本推送到新创建的远程仓库）

首先，创建一个空的远程仓库

将本地仓库关联到远程仓库：打开TortoiseGit设置,打开git远端，输入远程仓库的url

![image-20231106161835938](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106161835938.png)

此时远端仓库就关联到我们的本地仓库，点击推送将本地仓库的代码上传到远端

### 6.如何加速下载github上的项目

由于github属于国外的网站，我们可以复制github的仓库地址，然后再gitee里导入github上的地址创建一个gitee远程仓库，导入完成之后将gitee仓库导入到本地仓库即可，这样的话就间接的从国内的网站上导入了github的地址，速度明显会快很多。

## 5、代码冲突

冲突发生场景：

假如两个用户同时将远程仓库A的代码拷贝到他们的本地仓库，用户1和2都在A的基础上开发了新的版本B和C，如果用户1先提交了新版本B后,远程仓库更新到了版本B,然后用户2提交版本C,由于用户2的版本C是在A的基础上进行更新，但此时版本A已经不存在，因此C提交可能会发生错误

![image-20231106174716215](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106174716215.png)

解决方法就是C需要重新拉取远程仓库，获取到远程仓库B，此时git会自动合并代码B和C，再次推送代码C,远程仓库C更新成功

以上代码能够拉取再合并成功，是因为两个用户之间修改的内容没有冲突，因此系统会自动合并两者的代码不会发生错误

如果两个用户修改的代码块存在重叠，系统拉取合并的时候git无法判断应该保留哪一个版本，因此会发生报错

你可以通过选择保留哪一个版本的重叠的代码，来解决冲突

如何减少避免冲突：

- 勤提交，不要做完了所有功能才提交，完成某一个小阶段、小步骤都可以提交版本。
- 勤推送，对于已提交的代码，尽快推送
- 勤拉取，至少每天上班开工前拉取一次，创建分支前拉取一次。

## 6、Git分支管理

Git的分支是让Git从众多版本控制软件中脱颖而出的特点之一。便用分支意味着你可以把你的工作从开发主线上分离开来，以免影响开发主线。

![image-20231106195657485](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106195657485.png)

Git 的分支，其实本质上仅仅是指向提交对象的可变指针。Git 的默认分支名字是 master 。 在多次提交操作之后，你其实已经有一个指向最后那个提交对象的 master 分支。 master 分支会在每次提交时自动向前移动。

### 1.创建分支

 默认主分支为master,我们可以在主分支上创建分支，也可以在提交的版本上创建分支

![image-20231106200031005](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106200031005.png)

查看创建的分支：版本分支图

![image-20231106200217972](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106200217972.png)

![image-20231106200240509](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106200240509.png)

### 2.切换分支

选择切换/检出，切换到目标分支时，会自动切换到该分支最近一次提交的版本

![image-20231106200324258](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106200324258.png)

Git中有一个名为 HEAD 的特殊指针，通过它可以知道当前在哪一个分支上。在上一步中，我们仅仅是创建了一个新的分支testing，并没有切换到该分支去。

head表示当前工作分支

![image-20231106201042153](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106201042153.png)

### 3.合并分支

![image-20231106202445765](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106202445765.png)

`fast-forward 快速合并`：直接将一个分支b1合并到未经修改的主分支master上

![image-20231106201904180](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106201904180.png)

 `Auto-merging 自动合并`：主分支和分支都做了修改，如果修改的部分不重叠，使用自动合并，不产生冲突；否者产生冲突的话，采用之前的代码冲突的方法解决

![image-20231106202100751](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106202100751.png)



### 4.删除分支

打开分支试图，在里面选择分支进行删除

![image-20231106202746777](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106202746777.png)

### 5.标签

Git 可以给仓库历史中的某一个提交打上标签，以示重要，比较有代表性的是人们会使用这个功能来标记发布版本( v1.0、 v2.0 等等)。创建标签非常简单，TortiseGit中选择创建标签 菜单，在弹出的窗口中填写标签名即可。

![image-20231106202901200](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106202901200.png)

我们可以通过检出，跳转到指定标签、分支、提交的版本代码

![image-20231106203054668](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106203054668.png)

### 6.远程分支

#### 6.1 本地分支推送到远程

在前面的章节Gitee的使用中介绍了远程仓库的使用，默认情况下只会推送 master 分支到远程仓库，如果其他分支也需要在推送到远程仓库，在推送时要注意选择本地分支和填写远程分支名称。

选择git同步

![image-20231106203259156](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106203259156.png)

![image-20231106203233794](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106203233794.png)

我们也可以远程拉去分支到本地

#### 6.2 拉取远程分支到本地

拉取远程的一个跟踪分支b1

![image-20231106203607314](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106203607314.png)

由于拉取的是远程的一个跟踪分支，并不能直接拉取到本地仓库，因此我们需要去创建一个同名的本地分支用来存储跟踪分支，这个时候本地分支才算真正拉取完成

![image-20231106203924833](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106203924833.png)

# 二、软件开发流程

- 找到想要参与的开源项目
- fork项目仓库到自己的账号
- 克隆仓库到本地
- 创建分支开发
- 推送本地分支到远程仓库
- 创建pull request
- 开源项目负责人审核代码，决定是否采纳

Pull Request 是一种机制，让开发者告诉项目成员一个功能已经完成。一旦 feature 分支开发完毕，开发者使用GitHub 账号提交一个Pull Request，它告诉所有参与者，他们需要审查代码，并将代码并入 master 分支。PullRequest 不只是一个通知，还是一个专注于某个提议功能的讨论版、

发送pull request请求将自己的分支合并到原开发的master分支

![image-20231106204742435](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231106204742435.png)

# 三、pycharm Git使用方法

csdn链接：[Pycharm | 一文掌握 Pycharm 中的 Git 操作 ( 超详细)_pycharm使用git-CSDN博客](https://blog.csdn.net/hxj0323/article/details/109208253)

