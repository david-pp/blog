---
title: 踏上游戏引擎之路
date: 2018-04-03 21:00:00
category : 游戏开发
tags: []
---

还记得刚开始上班的第一天，手里抱着DirectX相关的几本书，准备好好学习一下关于哪些炫酷画面的单机游戏是如何做出来的。结果小组组长拉着我和刚来的一位同学到会议室，面前摆在一张关于服务器框架的图，一句话：“团队暂时不缺客户端，你们做服务器吧！”。当时都懵掉了，大学期间找好工作后，为了准备实习，都是在自学Windows操作系统原理和DirectX，Linux只是简单会装系统，简单的玩玩而已。我表明了自己的状况后，组长说了句：“没事，只要会C++就可以的”，就这样开始了游戏服务器端的开发，一晃就是十年。

如今，当年的冲动和激情仍在涌动，每每看到哪些精美的游戏画面，都会被深深吸引，想要一探究竟。本周走马观花地阅读了一遍《游戏引擎架构》，被游戏引擎涉及到知识量深深地震惊了，游戏开发的精华和激动人心的部分，原来都在引擎里面。幡然悔悟，原来自己也就只能算个伪游戏程序员罢了。

<!--more-->

《游戏引擎架构》这本书，涉及的面比较广，每个知识点深度一般，全书都是围绕这下面这张引擎知识框架图展开。

![游戏引擎架构](/images/game-engine-architecture.jpg)

不同层级的知识量都是惊人的，凭记忆大概介绍下：

 - Hardware(硬件)：
   游戏所运行的硬件主要涉及到的核心硬件那当然是GPU了，GPU据我所知它的计算方式和CPU有非常大的区别，GPU的架构是基于向量运算的，对于图形图像的数据处理起来要高效许多，也被大量用于科学计算。现在包含GPU的设备如普通的PC、游戏主机、手机，现在的性能都非常牛。

 - Drivers(驱动)：
   通往OS和硬件的中间层，就是驱动。键盘、鼠标、显示器都是需要驱动程序才能进行高级编程。

 - OS(操作系统)：
   说起操作系统也是五花八门，PC端如：Windows、Linux、Mac等，手机端如：iOS、Andriod、Windows phone等。做游戏开发，涉及到性能问题，如果对操作系统原理和相应的平台有不了解，那会死的很惨。

 - 3rd Paryt SDKs(第三方SDK)：
   游戏引擎的最基本的编程元素，图形渲染如：DirectX、OpenGL、貌似还有一个最新的叫Vulcan。C++基本库：STL、Boost、还有超级炫技的Loki。

 - Platform Independence Layer（平台独立层）：
   硬件、OS、图像渲染各种都是五花八门，为了基于引擎开发的游戏能够运行在这些平台下，就需要封装一层用于隔离这些依赖。花样又多了：平台侦测、原子数据类型、容器和迭代器、文件系统、网络传输、高精度定时器、多线程、图形渲染的封装、物理引擎的封装等。看到这一层有种想哭的感觉，C++功力不够，多个平台不了解，看懂尚需时间，自行封装简直要命。各种兼容性，搞不死也半残。

 - Core Systems(核心系统)：
   该层基于平台独立层提供的接口，进一步封装游戏引擎需要的核心模块。如：模块的启动和终止、断言机制、单元测试、内存管理、数学库、字符串库、语言的反射机制、异步IO等等。
 - Resources(资源管理)：
   游戏涉及的资源也是五花八门，号称第九艺术真不是吹的，纹理、材质、3D网格、视频、音频、字体、碰撞和物理参数等等，都需要非常好的工具和机制才能分管好线下和运行时的资源。

 - Low Level Render(低阶渲染)：
   说到这里，准备好积累多年的数学知识开始要用上拉，再往上层走，再次领略数学的重要性和自己的无知。图形图像的知识早都还给老师了，如何是好。光栅图形渲染、纹理和材质的渲染、静态动态光照模型、摄像机、图元剔除等等名词。

 - Scene Graph(场景)：
   2D、3D场景的绘制、各种神奇的框架分割数据结构和导航算法。

 - Visual Effects（视觉特效）：
   粒子、贴花、HDR光照、动态阴影、后期效果等等。

 - Front End (前端)：
   绘制完场景、和各种模型后，需要覆盖在其上一层UI、菜单等。

 - Skelton Animation(骨骼动画)：
   角色动画的基本方法，骨骼节点加蒙皮，至于算法和空间变换，现在脑子里面还说一片浆糊。一个有趣的IK（反向动力学）解决3D角色在与场景或其它模型互动时如何进行完美贴合，比如一个3D角色去开门，踩在起伏不平的地表上，手和脚如何完美贴合，没有违和感。算法挺神奇，对我来说暂时也是一头雾水，涉及到的数学真是尼玛太复杂了。

 - Collion & Physics(碰撞检测和物理引擎)：
   碰撞检测还好理解，物理引擎简直太逆天，理论知识涉及到刚体动力学，瞬间觉得大学物理都是渣渣。一座大桥被炸毁，漫天石块，最终落在地表上，角色模型走上去还得咯噔一下。自己体会下，涉及到碰撞和物理引擎。这一模块，没敢深究，以后要做游戏，物理没学好的情况下，物理引擎自动选择放弃。

 - Gameplay Foundation(游戏性基础)：
   游戏性的基础模块，如：消息/事件机制、脚本系统、静态世界元素、动态模型数据等等。

临了，作者写了一句，这仅仅是开始，尚有大部分内容还没有写进去，如：音频、视频、多人网络、AI等等。看完之后，整个人都不好了，做了这么多年游戏，原来还TMD没有上路。知耻而后勇，看完此书对游戏引擎的大体构架有了大概的认识，剩下的道路还有很长，每个知识点都需要深入学习才能研究透彻。

《游戏引擎架构》这本书对于我等新手来说，最重要的意义我认为是下面两点：

  - 指明了游戏引擎学习的道路。对游戏引擎的各个模块和元素都做了简单介绍，让新手脑子里面有一个大的框架和方向。
  - 研究某些游戏引擎的时做类比。拿到一个开源或商业引擎，不至于懵逼状态，可以根据上图来研究该引擎，不明白的再回来翻此书，进而查阅更多的资料。

就由这张图开始我的游戏引擎之路吧！作为一个菜鸟重新上路！