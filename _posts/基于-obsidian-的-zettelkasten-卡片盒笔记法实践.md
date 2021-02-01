---
title: 基于 Obsidian 的 Zettelkasten 卡片盒笔记法实践
siteurl: 基于-obsidian-的-zettelkasten-卡片盒笔记法实践
author: YJ2CS
avatar: /custom/avatar.webp
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 文章
tags:
  - 悦读
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-2021-01-03-星期日-131838
date: '2021-01-03T13:18:38+08:00'
date updated: '2021-01-03T14:41:51+08:00'

---

> Core passions and aspirations should be consistent and in sync.
> — <cite>Lorii Myers</cite>

TLDR 太长不读版本：

> - 1️⃣ 第一步：必须用自己的话写笔记卡片，以确保你将来能够理解。
> - 2️⃣ 第二步：无论何时添加新笔记，主动查找可链接到的已有笔记。
> - 3️⃣ 第三步：通过添加新记录并联系起来，延续这一系列的连续思考。
> - 4️⃣ 第四步：使用 Anki 间隔重复加深记忆，主动由大脑触发远程联想。

---

![webp](/images/c5d380325c104d7c4c7fc4de9b6867f7.jpeg)

Zettelkasten — How One German Scholar Was So Freakishly Productive ( <https://writingcooperative.com/zettelkasten-how-one-german-scholar-was-so-freakishly-productive-997e4e0ca125> ) 这篇文章炒鸡无敌棒，德国社会学家 Niklas Luhmann 卢曼使用 Zettelkasten 记笔记方法写了 70 多本书和 400 多篇学术文章。简单来说，Zettelkasten 是比加标签更高级的一种笔记方法，且听我给你费曼一下。😂

_**1**_

##

**Zettelkasten 卡片盒笔记法的背后缘由**

![webp](images/2146c7ca2792b38622e863c953c4db85.jpeg)

如果我们以文件夹、或文件（A4 纸）的形式来组织我们的笔记，总会遇到这样一个问题：所有笔记会铸成一种僵硬的结构，不可能重新排列。（可能是按照书本目录组织，或者是随着时间不断新增按照时间线组织，对吧？）

![webp](images/07a47646cd264b5647e9dee4beab5ecc.jpeg)

所以第一步，我们需要把每个块（chunk）打散，其实这些笔记本来就应该是自由浮动的，但问题随之而来：我们就不可能追踪它们之间的相互关系了。

![webp](images/3d270d5bf258c71df8054959e47fe4ad.jpeg)

为了表达每个知识点之间的关系，大部分人都采用了文件夹或者列表的方式，把这些卡片分门别类归档到不同的文件夹里面，比如印象笔记的笔记本，或笔记本组。

但其实，基于文件夹的方法很难在不同文件夹中归档的想法之间建立联系，特别是现实世界的复杂问题，其实不可能在某一个领域（即文件夹）内找到合适的答案；另外一个方面，这大大限制了所谓“创意”/“灵感”的产生，而我们现在常常说要“跨界”，对吧？

![webp](images/58b85078072bb430bedb8be03aa0ad69.jpeg)

那没关系呀，我加上标签不就行了嘛？标签是一个很大的改进，而且现在大部分 App 都已经支持。但事实上，每个标签有成千上万的笔记，我个人经验是收集的笔记打上标签之后，就再也没用到过了，还不如没有任何组织的时候，每个标签只会产生一大堆乱七八糟的东西。

![webp](images/8acccf98b906e4c251090a7f4827f98c.jpeg)

所以更好的办法是：我们应该用 Web（网络结构）来组织我们的笔记 —— 不仅仅依靠标签，还要把所有笔记连接在一起。

_**2**_

##

**Obsidian：提高认知能力的外部辅助设备**

唐·诺曼，全球最具影响力的设计师，《设计心理学》的作者，苹果的前用户体验架构师，在他的《Things That Make Us Smart 让我们变聪明的事情: 在机器时代捍卫人类属性》一书中提到：“人们高估了独立思考的能力。没有外部帮助，记忆、思维和推理都会受到限制。 (... ...)真正的力量来自于设计能够提高认知能力的外部辅助设备。”

所以接下来，我来介绍一款神器，Obsidian.app ( <https://obsidian.md/> ) 编辑器（借鉴于 Roam Research），相较于 Roam 最大的好处就是：能够跟我基于文件系统的本地化任务管理和知识管理系统相结合。

![webp](images/20bfc4737918ace3dcb092178cafea82.jpeg)

于是我创建了一个卡片笔记 [[提高认知能力的外部辅助设备]]，特别想要提醒你注意的是文中的紫色部分，[[唐·诺曼]] 和 [[Zettelkasten]]，当然还有右边标注的 Backlinks 部分。

1️⃣ 第一步：首先，使用 Zettelkasten 方法迫使你写作。特别是，根据这种方法，你必须用自己的话写笔记，以确保你将来能够理解它们。任何一个写过东西的人都知道，把东西写下来会迫使你把模糊的概念变成清晰的想法。

2️⃣ 第二步：其次，无论何时添加新笔记，使用 Zettelkasten 方法要迫使自己查找可以链接到的已有笔记。比如上面的 [[唐·诺曼]] 和 [[Zettelkasten]]。这样可以拓宽你的思维，迫使你去思考新的想法和你以前遇到过的其他想法之间的关系。

[[唐·诺曼]] 和 [[Zettelkasten]] 都是我之前创建过的词条，此时就能把不同卡片中连接在一起了，回忆一下上面的 图五：Web（网络结构）示意图。

![webp](images/5296f217d9998048e9467009bffd7138.jpeg)

_**3**_

##

**前方高能预警！卡片网状链接图！**

![webp](images/8de2f0d3c69c179852f56e9f514148a7.jpeg)

当我回到原来的 `[[Zettelkasten]]` 卡片笔记时，我能够看到 `[[Zettelkasten]]` 在整个卡片网络中的位置（①），以及其相互关联的卡片；② 即卡片主体；最右边 ③ 则是跟这张卡片相关的 双向链接（Backlinks），从而这就触发了笔记的第三步。

3️⃣ 第三步：Zettelkasten 可以储存一系列想法。Zettelkasten 就是关于连接想法的。因此，今天你可以有一系列的想法，把它们作为一系列相互联系的记录存储在你的卡片盒当中；然后，在未来的任何时候，你可以通过添加新的记录并将它们与以前的记录联系起来，延续这一系列的思考。

![webp](images/d131ce79e0788b8be880cca4fbf71436.jpeg)

而且，除了我在第二步当中已经绑定好的 `[[]]` 双向链接，它还能发现 Unlinked mentions 即未主动链接的想法（文本自动链接），从而我可以选择是否主动双向链接。当然这个时候是最有趣的，因为这主动触发了我之前的卡片笔记，从而以一种新的方式链接到了一起，有一种 🧠 大脑被联通的感觉，类似神经元的连接方式。

_**4**_

##

**使用 Anki 间隔重复增强复现机遇**

4️⃣ 第四步：其实我还有 Anki 处理的第四步，刚刚描述的是新建卡片和如何链接卡片，其实对于卡片本身来说，我会用 Anki 来特意间隔重复加深记忆，从而在写卡片时能够更加主动地由大脑（而不是由  Zettelkasten 编辑器）来触发我的远程联想。

间隔重复绝对是被认知科学证明是有效的，而对于一个笔记系统来说意料之外的不断重复即 See-Also 其实就是一个概念的社交场，我们寄希望于不同的知识点能够相互关联又相互碰撞，在合适的时候出现在我们的面前，唤起或发现可能潜在的一些联系。

通过 mdanki 自动把卡片笔记转化为 anki.apkg 格式并自动打开，即可导入 Anki，然后就可以尽情重复和背诵啦~ ✌️

    cdd #alias "cdd=cd ~/Library/Mobile\ Documents/iCloud~co~fluder~fsnotes/Documents"mdanki Zettelkasten.md ~/Desktop/Anki\ QA\ 笔记本/mdanki.apkg && open ~/Desktop/Anki\ QA\ 笔记本/mdanki.apkg

![webp](images/81a313f37b92ec86fc778812a5f1b5dc.jpeg)

_**5**_

##

**黑曜石，快来试一试吧！**

以往大家好像习惯了整理是基于分类，但其实那仍然是没有整理的笔记，不是有连接的笔记。而 Zettelkasten 卡片盒笔记法确实是构建了一个类似互联网的 Web 网状结构，不断点击链接可以了解更多。在大脑中穿梭，是一种很神奇的感觉，不过期待你的亲自体验。

来，Obsidian.app ( <https://github.com/obsidianmd/obsidian-releases/releases/download/v0.6.5/Obsidian-0.6.5.dmg> ) 安装包给到，你可以尝试一下。

![webp](images/cc4724dbc51ccef219931fc9ef5eacda.jpeg)

或者试一下 online 编辑器：https://roamresearch.com/#/app/Note-Tasking ( <https://roamresearch.com/#/app/Note-Tasking> )

还有国内同款：https://roamedit.com/jx/home.php?plugin=outline/sandbox ( <https://roamedit.com/jx/home.php?plugin=outline/sandbox> )

_**6**_

##

**再抛个问题：你觉得时间管理的终点是什么？**

就是什么情况下，你就不会再继续研究新的时间管理方式了？—— 这是一个来自好友的问题。

当然，还可以加上一个知识管理，跟时间管理一直是一对好兄弟。少数派 Hum 曾经在某一期 Podcast 里面提到过 “提升阶级” 是时间管理最重要的事情，也许吧。之前看物品整理，书中提到物品整理是有终点的，只要将每类物品确定了摆放位置，是个一次性的事情。固定物体存放的位置，物体移动完毕，用完放回原位，就不需要无限学整理收纳了。

我敢说整理绝不困难，是因为整理的对象都是物品。丢弃物品，移动物品，这是谁都能胜任的简单工作。除此之外，整理一定会有个终点。当你拥有的所有物品都有了固定的位置时，你就达到了整理的终点。—— 《怦然心动的人生整理魔法》

其实我觉得日本的书可能有个问题，就是人均居住空间太小了，只能整理整理再整理 🌝 （只是提供一个可能的角度，没有半点揶揄的意思）。还有从欧美流行过来的极简主义也是，都是因为美国/西方国家中产阶级近 30 年收入水平毫无提高，然后才不得已寻求的极简之道，参考：【Netflix】极简主义：记录生命中的重要事物 官方双语字幕 Minimalism ( <https://www.bilibili.com/video/BV1xE41147ak?from=search&seid=3314784422011127111> )

当然这是不得忽视的大背景，我只是批判性得思考了一下。中国 30 年改革开放，人均收入和幸福感，以及居住环境，不知道好到哪里去了。只是说现在经济增长又陷入了瓶颈，大家都很焦虑很浮躁，又开始寻求断舍离或极简主义之类的心灵滋养，当然不是说好与坏，而是认识到这一点之后我会觉得自己就没那么浮躁了。

关于时间管理，其实我更愿意按照稻盛和夫的方式去思考吧，就是一场修行。而这一步步修行，就是 格物、致知、诚意、正心、修身、齐家、治国、平天下。

![webp](images/d146c0996b5e19f79a884384e126f13a.jpeg)

可能就是要找到真正的志向，才是时间管理的终点吧。

否则，说白了就是，闲的！（😂 闲得你整天折腾这些。）

_**7**_

**参考资料**

> - How to take smart notes，方法及工具 - 少数派 ( <https://sspai.com/post/60466> )
> - Zettelkasten — How One German Scholar Was So Freakishly Productive ( <https://writingcooperative.com/zettelkasten-how-one-german-scholar-was-so-freakishly-productive-997e4e0ca125> )
> - 一个笔记系统——如何把 MarginNote3, DEVONThink3, TheBrain11 和 nvALT 当一个 App 使用 ( <https://www.bilibili.com/video/BV147411A7CP> )

来源链接：https://mp.weixin.qq.com/s/zCAGWzhMeo2wOjoQ9Dp0Kg
