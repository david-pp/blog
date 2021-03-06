---
title: 基于序列帧的2D角色动画
date: 2018-05-29 21:00:00
category : 游戏开发
tags: []
---

2D RPG游戏里面玩家扮演的角色，是非常核心的游戏元素之一。角色的外形、动作、装备、技能千变万化，玩家可以自由搭配。本文就由一个简单的RPG角色动画开始，对比了序列帧动画和骨骼动画，最后给出一个基于序列帧的简单设计和实现。

<!--more-->

### 1. 一个简单的RPG角色动画

玩家扮演的角色又称为Avatar（纸娃娃），可以通过操作装备或不同的技能展示出不同的动画效果。下面是一个简单的角色使用攻击技能的动画， 主要由下面几部分组成：

- 外形：一个女性角色。
- 翅膀：绿色的翅膀。
- 武器：斧子。
- 技能：斧子砍下去的火焰特效。

![演示](/images/avatar_showcase.gif)

（PS. Avatar的确也是电影阿凡达的英文，若翻译成“替身”显得有些Low啊。哈哈）

### 2. 序列帧动画 vs. 骨骼动画

<!-- ** 序列帧 vs. 骨骼动画：** -->

2D游戏角色动画的实现一般分为两类：序列帧动画、骨骼动画。

**序列帧动画：**

序列帧动画就是利用一张张的图片按照一定的帧率进行播放即可，实现简单，运行效率高。

下面用斧子砍的动画，可以拆分为三个部件：角色外形、武器、技能。

![演示](/images/avatar_showcase3.gif)

外形动画序列：

![序列帧](/images/avatar_act.png)

武器动画序列：

![序列帧](/images/avatar_weapon.png)

技能动画序列：

![序列帧](/images/avatar_skill.png)

合在一起，使用固定的帧率同时播放即可，每一帧三种类型的图片位置比较对应好。当然，帧率和位置信息一般都是美工同学绘制时通过工具生成的。

**骨骼动画：**

骨骼动画，只保存各部分切图组成的纹理集和动画数据，只需要极少的原画，便可完成千变万化的动作动画组合。在动画制作时，只需要完成对关键帧的编辑，通过动画补间，便可自动生成流畅的动画动作。

![骨骼动画](/images/avatar_bone.png)

**序列帧 vs. 骨骼动画：**

骨骼动画听起来是完美的角色动画实现方案，对于3D游戏也是唯一的实现方案。对于2D游戏来说，骨骼动画不一定是最适用的方案：

- 动作细节缺失：骨骼动画说白了就是使用一些部件运行时进行旋转、缩放、平移，靠数学运算生成一帧，势必会让角色动作中的细节缺失。

- 运行性能不如序列帧：骨骼动画的一帧是由多个部件拼凑成的，每个部件对应一个纹理对象，运行时的Draw Call会明显高于序列帧。序列帧方式，每一帧都是一个纹理对象而已。

- 资源占用极小是最大的优势：序列帧方式会使用大量的图片资源，动作稍有不同就得绘制不同的美术资源。

对于2D游戏，如何选择？下面给一个简单的选择标准：

- 写实类，动作复杂，细节要求较高，选择序列帧方式。（一般2.5D RPG游戏都会选择使用序列帧方式，2.5D角色方向一般都是8个，骨骼做起来也比较麻烦）
- 卡通类，动作简单，角色方向单一，可以选择骨骼动画。（什么机甲之类的最适合了，动作本来就比较机械）
- 空间换时间，还是时间换空间？

### 3.基于序列帧的设计

下面基于序列帧做一个简单的设计。假设角色状态如下：

- 方向：当前的方向，一共有八个方向。
- 动作：站立、移动、攻击。


- 外形：角色外形。
- 武器：手里拿的武器，刀、斧等。
- 技能：攻击时的特效。
- 翅膀：增加表现力的翅膀。

注意：外形、武器、技能、翅膀这些绘制的时候，根据情况针对不同的方向和动作都要准备相应的图。所以，添加新的动作时，相应的部件可能都需要提供一套图片资源。


** 角色方向：**

八个方向如下。为了节约资源，也可以仅制作0-4五个方向的图，方向5、6、7分表使用3、2、1的图进行翻转，当然要考虑翻转后效果是否满足画面要求。（比如，双手武器，来个翻转就不协调了）

![角色方向](/images/avatar_flip.jpg)

**资源命名：**

更加动作和方向，不同的部件需要提供多张图片资源。制定一个好的命名规则很有必要，比如：

```
类型-编号-动作-方向
```

假设当前角色正在进行攻击，方向是1。那我们就需要取下面的动画序列进行播放即可：

- player-100-attack-1.png
- weapon-200-attack-1.png
- wing-300-attack-1.png
- skill-400-attack-1.png

**资源目录：**

针对不同的资源类型，可以分不同目录，或者一个资源数据库的形式都是可以的。下面只是一个简单的基于目录的示例：

```
avatar
   |- roles
   |     |- player
   |     |     |- p-x-stand-0.png
   |     |     |- p-x-move-0.png
   |     |     |_ p-x-attack-0.png
   |     |_ npc
   |- weapon
   |     |- w-x-stand-0.png  
   |     |_ w-x-attack-0.png  
   |- wing
   |     |- wing-x-stand-0.png  
   |     |_ wing-x-attack-0.png  
   |- skill  
   |    |- skill-x-attack-0.png    
   |  
```

### 4. 基于Egret的演示

Egret提供了MovieClip来播放动画，基于MovieClip实现一个简单Avatar系统。

代码位置：

[https://github.com/david-pp/egret-code/tree/master/demo_avatar](https://github.com/david-pp/egret-code/tree/master/demo_avatar)

效果如下：

![演示](/images/avatar_showcase2.gif)
