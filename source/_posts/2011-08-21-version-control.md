---
layout: post
title: 版本制作自动化方案
category : 项目管理
tags : [开发过程, 版本管理]
date: 2011-08-21 12:49:00 +0800
---

最近工作比较忙，游戏功能相关的代码就敲个不停，根本无暇去思考一些东西。趁着周末，整理下上周关于版本制作流程的优化的一些想法，有时间就立马去实现，希望能提高征途2项目组做版本的速度，省得每次程序兄弟每天等版本等的都想哭，还有做版本的兄弟，天天就重复着在一堆svn revision list里面挑来挑去，如此机械的工作，希望脚本可以帮他们完成一部分。既省了时间，也省了人力。只期望以后少加点班。

### 1. 问题

有两个svn分支，一个用于开发（主干Trunk），一个是对外发布的分支（BranchA）。根据要发布的内容列表，合并主干相关的revision到BranchA，有冲突解决之。

这里，要发布的内容列表就是taskid列表，某个要发布的功能都会对应唯一的一个taskid，代码提交的时候，提交日志里面一般都会标有相对于的taskid。制作版本的核心，就是根据这些taskid从主干代码提交日志里面去找相应的日志条目所对应的revision，然后合并到BranchA，合并过程中有冲突则找相应的程序予以解决。重复上面的操作，直到tasklist里面所有功能相关的revision都被合并入BranchA。然后提交，编译，测试。

### 2. 版本制作现状

手动维护一份未从Trunk分支合并到BranchA分支的revision列表，Trunk新提交的日志也只能手动复制下来，格式化处理一下，然后复制到未合并列表中。根据需求，将要发布的功能，一个一个在未合并列表里面手动查找，然后去合并，有冲突就通知相应的开发程序去解决冲突，合并好之后把相应的条目从未合并列表中删除。全部手动完成的，机械并且效率低下，并且很容易漏掉某条提交revision，实在于心不忍啊！

### 3. 解决方案

对于上述问题和现在版本制作现状，提出下面的解决方案。实现制作版本的脚本化，提高工作效率。

注意：版本制作的自动化，刚开始实施可能会有点问题。因为大家的提交日志，虽然包含了功能taskid，但格式千奇百怪，没有统一，虽然脚本可以识别大多数提交日志，但还是存在挑错revision的可能，对此也不必担心，在此提供了一个工具去检测这些不符合规格的提交日志，可以先手动去改下这些提交日志即可。希望以后svn提交日志的格式也符合指定的规格，这样更加利于版本制作的自动化。

#### 3.1 版本工具脚本

**config**

用于配置一些相关参数，比如svn主干或分支的路径、未合并revision列表文件、task列表文件、版本制作日志等等。

**getlog** 

根据指定的revision或revision1:revision2从主干上抽取相应的日志，并格式化处理之，格式化处理的文件有3种格式，下面会有详细介绍。

**pickup** 

根据要合并的功能列表文件（tasklist）从未合并列表unmergelist中挑选出要合并的revision列表，并将其从未合并列表中删除。生产一个要合并的revision列表文件mergelist，和新的unmergelist文件。

**merge**

根据mergelist提供的revision，将主干Trunk中相应的revision合并到BranchA中，没有冲突就自动提交并继续下一个revision，有冲突就会停止并给出提示，此时可以让相应的提交者解决冲突。

**resolve**

冲突手动解决完后，执行该脚步，自动解决所有的冲突然后提交到BranchA，继续执行merge脚步直到结束。

#### 3.2 三种版本日志格式

* list  revision数字列表文件。
* xml xml格式的提交日志，提供了详细的提交信息，版本号、提交者、日期和时间、提交日志等。
* txt  文本格式，只提供版本号、提交者和提交日志。

![三种文件格式的转换](/images/2011-08-21-1.png)

#### 3.3 整体流程

制作版本的整体流程图如下：

1. ready 准备阶段，设置配置。
2. getlog 提取主干日志。
3. pickup 挑版本。
4. merge 合并。
5. resolve 解决冲突。

![整体流程](/images/2011-08-21-2.png)


##### 3.3.1 准备工作（ready）

执行所有脚本之前，请先做好准备工作。详细内容如下：

* 准备好当前的未合并列表文件（unmergedlist）
* 准备好要发布的功能列表文件（tasklist）
* 创建一个工作目录，脚本生成的结果和中间结果都会在此目录下
* 配置config脚步里面相关的参数，包含svn路径、工作目录等等相关信息。

![准备工作](/images/2011-08-21-3.png)

##### 3.3.2 提取日志（getlog）

从主干中提取指定版本范围的提交日志。此操作根据需求可选。生产的gelostlist文件的路径还有版本范围都在config配置中可进行设置。

![提取日志](/images/2011-08-21-4.png)


##### 3.3.3 挑选版本（pickup）

完成版本挑选过程，代替人工挑选版本的机械重复劳动。

![挑选版本](/images/2011-08-21-5.png)

##### 3.3.4 合并版本（merge）

自动完成版本的合并工作，并提交至BranchA。有冲突就会停止，手动解决完冲突后继续执行该脚步。合并完所有mergelist里面的revision。

![合并](/images/2011-08-21-6.png)


##### 3.3.5 解决冲突（resolve）

merge的时候发现冲突了，让相应的提交者修改完冲突后，执行该脚本，完成解决冲突的工作并提交至BranchA。然后继续执行merge脚本。

![解决冲突](/images/2011-08-21-7.png)


### 总结

希望上面的脚本可以实现版本制作的自动化，上面的内容不包含编译、打包、测试等过程。对于版本制作者而言，上面这些工作占据了大量的时间，并且非常容易出错。希望该方案可以解决版本制作中的一些问题。



