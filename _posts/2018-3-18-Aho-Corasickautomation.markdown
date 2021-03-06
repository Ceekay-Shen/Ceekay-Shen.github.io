---
layout:     post
title:      "AC算法详解"
subtitle:   " \"smb server, smb setup\""
date:       2018-03-18 10:00:00
author:     "Shen"
header-img: "img/home-bg-art.jpg"
catalog: true
tags:
    - 计算机
    - 服务器
    - smb
    - Linux
---

> “字符串匹配算法核心”

# AC多模式匹配算法

## 前言

因为工作原因（DPI：深度报文检测），所以接触最多的算法便是字符串匹配算法，如AC、BM、Pcre算法，其中最重要的便是AC多模式匹配算法，算是对其有一点浅显的理解，故将其记录下来。

## AC算法简要介绍及其原理

AC算法是Alfred V.Aho（《编译原理》的作者），和Margaret J.Corasick于1974年提出（与KMP算法同年）的一个经典的多模式匹配算法，可以保证对于给定的长度为n的文本，和模式集合P{p1,p2,...pm}，在O(n)时间复杂度内，找到文本中的所有目标模式，而与模式集合的规模m无关。

AC算法实现的原理，大体上可以分为三部分： 
+ 状态跳转表（goto表）：决定对于当前的状态S和条件C，如何得到下一个状态S`。
+ 失败态跳转表（faile表）：决定goto表得到的下一状态无效时，应该回退到那一状态。
+ 匹配结果输出表（output表）： 决定在哪个状态时输出哪个恰好匹配的模式。  
**其中最重要的是如何构造faile表。**

以模式串pattern = {"he","she","his","hers"}来演示如何构造上述的三张表。

##构造goto表： 

首先规定一个初始状态0，接着一次处理多个模式字符串。  
+ 首先是“he”
![image](/img/post-ac/he.png)
图中每个圆圈代表一个状态（state），圆圈中数字表示状态的编号；有向箭头表示状态的转换，表示由当前状态指向下一个状态；箭头上的字符表示此次状态的转换的条件。
+ 接下来是“she”
![image](/img/post-ac/she.png)
每处理一个新的模式串的时，都先回到起始状态0： 
+ 然后是“his”
![image](/img/post-ac/his.png)
如果给定条件C，从当前状态出发能转换到一个有效的状态，那么只进行状态的转换，而不创建新的状态。如创建“his”模式串的状态机时，从0状态输入字符h能到一个有效的状态1。 

+ 最后是“hers”
![image](/img/post-ac/hers.jpg)
AC算法还规定，当当前状态是初始状态（0）时，对于任意条件C来，都能转换到有效的状态，因此除了条件‘h’和‘s’外，对其他任意的条件，状态0还应转换至状态0；

如上goto表已构建完成，其有效的翻反应了有效的状态转换。

##构造failed表：

fail值的求法可以用递推法来进行。  
首先规定与状态0距离为1（即深度为1）的所有状态的fail值都是0。
即fail(1) == 0，fail(3) == 0。  
然后设当前的状态为S<sub>5</sub>，求fail(S<sub>5</sub>)。S<sub>5</sub>的前一状态必定是唯一的（从goto表中可以看出），设S<sub>5</sub>的前一状态是S<sub>4</sub>，S<sub>4</sub>转换到S<sub>5</sub>的条件为C，测试S<sub>5</sub> = goto(fail(S<sub>4</sub>),C)，如果成功，则S<sub>5</sub> = goto(fail(S<sub>4</sub>),C)，如果不成功，继续测试S<sub>4</sub> = goto(fail(S<sub>3</sub>),C)是否成功，如此重复，直到转换到某个有效的状态S<sub>n</sub>，令fail(S<sub>5</sub>) = S<sub>n</sub>。  
如下是求各个状态的fail值
fail(2) == goto(fail(1), e) == goto(0, e) == 0  
fail(4) == goto(fail(3), h) == goto(0, h) == 1  
fail(5) == goto(fail(4), e) == goto(1, e) == 2  
fail(6) == goto(fail(1), i) == goto(0, i) == 0      
fail(7) == goto(fail(6), s) == goto(0, s) == 3  
fail(8) == goto(fail(2), r) == goto(0, r) == 0  
fail(9) == goto(fail(8), s) == goto(0, s) == 3

|状态|0|1|2|3|4|5|6|7|8|9|
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|fail值 |0|0|0|0|1|2|0|3|0|3|

##构造output表

在构建goto表的过程中，知道状态2、5、7、9是输入的4个模式串的末尾部分，所以如果在执行匹配的过程中，达到了上述四个状态，就知道对应的模式串被发现了。此时用output表记录这一信息。完成goto表的构建后，我们所得到的output表如下：
+ 2 he
+ 5 she
+ 7 his
+ 9 hers

但是此时并不是完整的output表。下面以构建fail(5)为例，说明下fail表的构建是如何影响到output表的。在计算fail(5)的值时，将模式she的所有包含的后缀提取出来，包括he,e。在output表中，状态5是一个输出状态。当用he在状态机中转移的时候，到状态2时，output[2]也是一个输出状态，意味发现模式串she的同时也发现了模式串he，则意味着发现了she和he两个模式，此时fail[5]=2,所以我们需要将output[2]所包含的输出字符串加入到output[5]中国。完成goto和fail表的构建的同时，也构建好了output表。

+ 2 he
+ 5 she he
+ 7 his
+ 9 hers

## AC算法匹配

具体步骤：
+ 从文本的待匹配字符的第一个text[i]开始，用text[i]从goto表的状态S[0]开始执行跳转。
+ 如果存在可执行的跳转方案goto[S[0],text[i]] = P ,P != 0,则i++，同时转移到状态S[P]。
+ 如果不存在可行的跳转方案，则考察状态S[P]的fail值，如果fail值不为0，则跳转到S[fail[P]]，再次查goto[fail[P],text[i]]是否等于0，直到发现不为0的转移方案或者对于所有经历过的fail状态对于当前输入text[i]都没有非0的转移方案为止，如果确实不存在非0的转移方案，则将i增加1，同事转移到S[0]继续执行跳转。
+ 在每次跳转到状态S[P]时，都需要查看下output[P]是否指向可输出的模式串，如果有，说明当前匹配了某些模式串，将模式串输出。