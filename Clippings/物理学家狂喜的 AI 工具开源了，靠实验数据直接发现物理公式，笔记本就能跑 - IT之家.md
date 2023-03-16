# 物理学家狂喜的 AI 工具开源了，靠实验数据直接发现物理公式，笔记本就能跑 - IT之家
[物理学家狂喜的 AI 工具开源了，靠实验数据直接发现物理公式，笔记本就能跑 - IT之家](https://www.ithome.com/0/680/205.htm) 

 一个让物理学家狂喜的 AI 工具，在 GitHub 上开源了！

它名叫 **Φ-SO** ，能直接从数据中找到隐藏的规律，而且一步到位，直接给出对应公式。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-16%2019-49-32/82aea290-6b6f-4fec-815d-2351abd7fdb9.png?raw=true)

整个过程也不需要动用超算，**一台笔记本大概 4 个小时**就能搞定爱因斯坦的质能方程。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-16%2019-49-32/bc8913b3-d8ee-46ae-b545-9853a6c118cd.png?raw=true)

这项成果来自德国斯特拉斯堡大学与澳大利亚联邦科学与工业研究组织 Data61 部门，据论文一作透露，研究用了 1.5 年时间，受到学术界广泛关注。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-16%2019-49-32/97c047a4-ecba-4176-aa6c-9e73a986d21f.png?raw=true)

代码一经开源，涨星也是飞快。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-16%2019-49-32/439b133b-fd85-4f71-b60c-efbf9c960e1d.png?raw=true)

除了物理学者直呼 Amazing 之外，还有其他学科研究者赶来探讨，能不能把同款方法迁移到他们的领域。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-16%2019-49-32/1cefba8f-7242-4fe2-9f56-2fd1de8b6045.png?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-16%2019-49-32/1d0474ec-4a6f-48f3-b0a6-eb504efdf68d.png?raw=true)

强化学习 \+ 物理条件约束
--------------

Φ-SO 背后的技术被叫做“深度符号回归”，使用**循环神经网络（RNN）+ 强化学习**实现。

首先将前一个符号和上下文信息输入给 RNN，预测出后一个符号的概率分布，重复此步骤，可以生成出大量表达式。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-16%2019-49-32/294a821c-374e-4857-a155-888b54205983.png?raw=true)

同时将物理条件作为先验知识纳入学习过程中，避免 AI 搞出没有实际含义的公式，可以大大减少搜索空间。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-16%2019-49-32/5ca329c6-e6d8-44c4-a730-1ba07c00af8b.png?raw=true)

再引入强化学习，让 AI 学会生成与原始数据拟合最好的公式。

与强化学习用来下棋、操控机器人等不同，在符号回归任务上只需要关心如何找到最佳的那个公式，而不关心神经网络的平均表现。

于是强化学习的规则被设计成，只对找出前 5% 的候选公式做奖励，找出另外 95% 也不做惩罚，鼓励模型充分探索搜索空间。

研究团队用阻尼谐振子解析表达式、爱因斯坦能量公式，牛顿的万有引力公式等经典公式来做实验。

Φ-SO 都能 100% 的从数据中还原这些公式，并且以上方法缺一不可。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-16%2019-49-32/6cece213-f798-4845-bfea-ae9cb0c5423e.png?raw=true)

与其他方法入 MLP 相比，Φ-SO 在训练范围之外的表现也要更好。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-16%2019-49-32/2ce7bbaf-2913-41b4-9e7d-b6337dba543e.png?raw=true)

研究团队在最后表示，虽然算法本身还有一定改进空间，不过他们的首要任务已经改成用新工具去发现未知的物理规律去了。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-16%2019-49-32/03f96440-fcc5-467c-9913-52f51b321227.png?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-16%2019-49-32/6f2aa7af-6150-473e-b749-bdb7c1a14783.png?raw=true)

GitHub：

https://github.com/WassimTenachi/PhySO

论文：

https://arxiv.org/abs/2303.03192

参考链接：

*   \[1\]https://twitter.com/astro_wassim/status/1633645134934949888
    

本文来自微信公众号：[量子位 （ID：QbitAI）](https://mp.weixin.qq.com/s/0eGiiWdKUyQcg54azY5evg)，作者：梦晨