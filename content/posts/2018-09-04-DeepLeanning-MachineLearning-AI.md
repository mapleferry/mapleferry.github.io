---
title:  "人工智能、机器学习与深度学习"
date: 2018-09-04T16:40:00+08:00
ShowToc: true
categories: [AI]
series: ['']
tags: ['AI', '深度学习', '机器学习','博客']
---

您可以将深度学习、机器学习、人工智能想象成一组由小到大、一个套一个的俄罗斯套娃。深度学习是机器学习的子集，而机器学习则是人工智能（AI）的子集。
![图片](/img/Deep_Machine_AI.jpg)

<!--more-->

广义上讲，任何能够从事某种智能活动的计算机程序都是人工智能。

它可以是一堆if-then语句，也可以是一个复杂的统计模型。每当人工智能研究者设计出一种擅长某种任务（例如下国际象棋）的计算机程序时，许多人通常都会说“这不是真正的智能”，因为人们完全理解其算法的内部原理。所以也可以说真正的人工智能是任何计算机目前还做不到的事。

人们常说，机器学习是人工智能的子集。这意味着所有的机器学习都能算作人工智能，但并非所有人工智能都属于机器学习。例如，符号逻辑（规则引擎、专家系统和知识图谱）、进化算法和贝叶斯统计都可以称为人工智能，但它们都不属于机器学习。

机器学习之所以包含“学习”二字，是因为机器学习算法会尝试优化一项特定的指标：它们一般会努力将预测的误差最小化，或者说将预测正确的概率最大化。这样的算法有三种名称：误差函数、损失函数、目标函数（因为这种算法有一个目标……）。如果有人说他们在用一种机器学习算法，那么只要问两个问题就能大致了解这种算法的价值：目标函数是什么？

怎样将误差最小化？一种方法是构建一个算法框架，通过与输入值相乘来预测输入的性质。不同的输出/预测结果是输入值与算法的乘积。初始的预测通常错误较大，如果您有和输入值相关的实际基准标签，就可以将预测结果与基准标签进行对比，衡量误差的大小，然后依据误差来调整算法。这就是神经网络的工作方式。它们会不断衡量误差、调整自身的参数，直至误差无法继续缩小。

简而言之，神经网络就是一种优化算法。如果调试得当，神经网络可以经由反复的预测来实现误差最小化。

深度学习是机器学习的一个子集。深度人工神经网络是一类在图像识别、声音识别、推荐系统等重要问题上不断刷新准确率纪录的算法。DeepMind声名远扬的AlphaGo算法在2016年早些时候击败了前世界围棋冠军李世石，这种算法当中就包含深度学习技术。

“深度”是一个术语。它指的是一个神经网络中的层的数量。浅层神经网络有一个所谓的隐藏层，而深度神经网络则不止一个隐藏层。多个隐藏层让深度神经网络能够以分层的方式学习数据的特征，因为简单特征（比如两个像素）可逐层叠加，形成更为复杂的特征（比如一条直线）。
