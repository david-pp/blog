---
layout: post
title: 每周回顾：无意义的加班
category : 每周回顾 
tags : []
date : 2015/09/18  20:00  +0800
---

周三晚上加班到周四凌晨三点多，差两个小时就可以看日出了。一直处于无事等待状态，这样算是幸运的了。若这个时候有代码修改，那估计日出看定了。

加班本该是赶进度的一种手段，偶尔为之也是可以理解的。天天加班都不说什么了，每周版本发布前夕非要搞到大家一起看日出，这样好吗？策划同学貌似零点过后状态奇佳，灵感一个接一个，测试同学不紧不慢地测着，相关程序则必须守着，多数时候，程序呆在哪里什么事情都没有，纯粹就是为了其他人安心而已。

<!--more-->

这种加班的直接代价，就是：


- 1> **修改代码出BUG的概率高**。都吭哧吭哧码了一天代码了，深更半夜哪里还有什么精力去考虑那么多，说改什么就改什么，若测试同学再疏忽一下，第二天的版本肯定出BUG。

- 2> **修改代码的时间成本高**。想象一下，面对百万行的C++代码，有时在一个小头文件里面修改一行代码，编译一遍就有得等了，编译完还需要部署更新，然后才能测试。完整做一遍版本也是会花不少时间的。所以，最不幸的就是晚上代码有修改，那真是雪上加霜，会拖的更晚。

- 3>  **出问题时相关人员都不在岗**。第二天版本发布后如果有问题的话，相关人员都不在，都看日出了，是人就得要休息不是。换人查问题效率会非常低。当然，也可以把相关人员从被窝里面拽出来，然后无精打采，精神萎靡，骂爹骂娘地帮你找问题，效率也不会高到那里去。


长此以往的代价，就是：

- 1> **作死**。人类进化了多少年，日出而作日落而息。加班到看日出，这不是逆天吗？

- 2> **积怨**。精神萎靡，心存抱怨，再正常不过的事情了。外面有更好的机会，不走更待何时？军心不稳，何谈攻城？


这样无意义的加班不是一次两次，而是长期如此，该反思了。我一直认为，加班是进度管理上的无能，一方面是自身进度管理能力不足，另一方面是因为领导并没有充分授权，没有权利拒绝一些无理的需求。还有一点，可能是人的劣根性，什么事情都要拖到最后，就像学生一般只会在考试临近的那一两周内加强复习，平时则不紧不慢的，这就就叫学生综合症。对于上面说的这种无意义的加班，一直都在提醒，制作版本放在白天进行，为什么每次都会拖到最后一刻？只能说是人性所为了。

--------------------------------

本周快成光杆司令了，离职的离职，休假的休假，什么事情都得上手了。所以，本周基本上都在忙工作，所以不分学习篇、工作篇了。


##### 数据解析工具优化

财务的同学反映，角色道具解析工具解析的道具名显示有乱码，只是一小部分角色的数据是这样的。在确认了数据的位置后，找运维的同学搭建测试环境，果不其然，有乱掉的。原因也很简单，对于那些流失的角色，长时间没有上线，道具数据结构有过变动，解析的时候没有考虑到兼容之前的历史数据。

还有一个令人费解的地方，就是数据库中有某些角色的道具某些字段有异常，可能是某些时间，某些程序的BUG导致，无从追踪了。这个就真无能为力了，因为时间太久了，无从查起了。

TIPS：在设计存档的时候，如果非要自己写，不用现成的库（如：protobuf,thrift等），可扩展性一定要做好！

##### 新功能开发

被领导周一晚上快23点的时候，叫到办公室，说有一个紧急任务，本周内开发完成并发布。当时策划案子还没有了，当时心里就颤了一下，心想：这周估计能看几次日出了。周二上午，案子草草地过了一遍，和执行策划遇到一个难点僵持不下，直到下午领导有空，一句话就搞定了，相视一笑，开始码代码。 还好我已经是熟练代码民工了，吭哧吭哧码了一天半，在周三晚上基本开发完成，就等测试了。和QA开玩笑的说，“今天准备好看日出吧！” ，结果告诉我计划变了，下周发布。我勒个去啊。。不过也好，多测试几天，稳定性第一，安全第一。

##### 查核日志

最烦的就是当你在做一件事情的时候，中间各种打断。有时可能态度也不太好，发火也正常。先对那些在这种情况下，被我喷的人们说声对不起，性格使然，还望不要计较，呵呵。

查日志是我们这些做后台的经常需要干的事情，验证角色的行为，查核一些BUG等等。往往时间很难确定，有时几分钟就可以搞定，有时查一天都没有结果。

1> 这一周因为机房割接导致有些游戏区全部掉线，并无法自动恢复，日志的结果看着是因为内部服务器之间的连接断掉导致，并一直无法重连，感觉是机器之间的网络不通，若要验证此感觉，要花费大量的时间去深究日志和底层代码，因为手头事情较多，就不处理了，割接这种事情也不是天天发生。之前并不知道割接是怎么一回事，于是google了一下：

> 网络割接是对正在使用的线路、设备进行操作，将会直接影响到上面承载的业务，网络改造中最关键的一步就是网络割接。网络割接又叫网络迁移，是指运行网络物理或者逻辑上的更改。


2> 产出和消费数据对不上。就在开发功能的时候，接到一个超高优先级的任务，于是查核了好几个区的相关日志，过滤导入到Excel，最终还是都找到了原因，给出了合理的解释和说明。

--------------------------------

这一周过去了，忙碌中过去了，习以为常的工作内容总让人感觉没什么收获。少一些加班，让我们生活更美好吧。只能再次呼吁一下了！

时间是我们有限的资源，工作并非一切，只是一种自我实现和赚钱的手段。我们还有自己的生活，还有家庭要照顾，还有嗷嗷待哺的宝宝需要大人的陪伴和共同成长，还有很多我们不能错过的东西。