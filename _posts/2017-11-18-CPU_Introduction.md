---
layout: post
title:  "CPU_Introduction"
date:   2017-11-18 14:20:09 +0800
catalog: true
tags:
    - 编程
---


# Intel & AMD
- 在1980年代时，英特尔在全球是前十大的半导体销售的业者（1987年是第10名），而在1991年以后，英特尔达到了第一名的位置之后就没有再变动了。而其次的 **半导体公司** 包括AMD、三星、德州仪器、东芝与意法半导体。而唯一在x86处理器市场中能够与Intel相抗衡的是AMD。英特尔（Intel）和超威半导体（AMD）(Advanced Micro Devices)占领了 **个人电脑CPU** 绝大部分市场。  
- Intel与AMD两家本是同根生，有着很深的渊源，他们的创始人都来自同一家企业——仙童半导体公司，甚至还有过密切合作。
- Intel的两位创业者,**诺伊斯** 是集成电路发明人之一，**摩尔** 则是著名的“摩尔定律”提出者，他们在创办Intel之前就已经是业界威望很高的名流，这些先天性优势是它在短时间内就能吸引到大量的优秀技术人才，在微处理器的研制上一开始就处于领先地位。反观AMD，其创始人桑德斯只是销售出身，创业之路举步维艰。正因为如此，AMD初期的定位很明确，以市场为导向，凭借质优价廉的产品努力成为各类产品的第二供应商。作为第二供应商要求的不是技术领先与创新能力，而是学习模仿以及生产制造能力，显然这与AMD当时的自身条件是匹配的。而Intel则以技术发展为导向，是典型的技术领先与创新者。
- 在1982年，IBM迫使Intel与AMD签署协议，使后者成为8086和8088处理器的第二供应商（因为身经百战的IBM知道，如果将微处理器完全放给一家供应商，很有可能造成其坐大难控）Intel开放技术，全面授权AMD生产x86系列处理器，而AMD则放弃了自己的竞争产品，成为Intel后备供应商。双方联手合作，终于拿下了IBM的订单，也由此锁定了个人电脑技术发展路径。


<img src="{{ site.url }}/pic/{{page.title}}/intelVsAmd.jpg" />
<div class="text" style="text-align:center;font-weight:bold"> Intel VS AMD </div>

## intel product series

<img src="{{ site.url }}/pic/{{page.title}}/interCpu.jpg" />
<div class="text" style="text-align:center;font-weight:bold"> Intel CPU  酷睿>奔腾>赛扬 </div>

## AMD product series

<img src="{{ site.url }}/pic/{{page.title}}/AMDCpu.jpg" />
<div class="text" style="text-align:center;font-weight:bold"> AMD CPU 锐龙>AMD FX>APU>速龙>闪龙</div>

# Intel Core 系列CPU
## 产品线
Intel Core（停产）、Core 2（停产）、Intel Core M（纤薄轻巧的2合1电脑）、Core i（i3、i5、i7、i9）

## Core i 历代架构（eg i7）
[wiki](https://en.wikipedia.org/wiki/List_of_Intel_Core_i7_microprocessors)  
- Westmere microarchitecture (1st generation)
- Sandy Bridge microarchitecture (2nd generation)
- Ivy Bridge microarchitecture (3rd generation)
- Haswell microarchitecture (4th generation)
- Broadwell microarchitecture (5th generation)
- Skylake microarchitecture (6th generation)
- Kaby Lake microarchitecture (7th generation)
- Coffee Lake microarchitecture (8th generation)

i7系列当中，大部分CPU的售价在200-600美元，而其中有小部分售价为999美元或以上（一般至尊版i7定价都是999美元，其象征意义远大于实际意义），这种价格较高的CPU被称为“英特尔® 酷睿™ i7 处理器至尊版（Intel Core i7 Extreme Edition）”。

## 至强（Xeon）
Xeon（读作/'zi:on/）是Intel的一个中央处理器品牌，中国译名至强，主要供服务器及工作站使用，亦有超级计算机采用此处理器。Intel XeonE3-1230曾因高性价比而受到电脑DIYer的热捧，有“i5的价格，i7的性能”的美誉。但随着英特尔的策略改变，使得E3-1230后续系列升价，而且主板数量少，价格也变贵，这个系列不再具有超值之选的吸引力。

## CPU处理器编号
处理器类别或家族中的编号越高，表明其特性越多，但也会存在因为某种特性较强，另一种特性则较弱的可能。如果您选定了某个特定处理器品牌和类型，可与处理器编号比较以确认该处理器包括您需要的功能。

[每一代的型号含义](https://www.intel.cn/content/www/cn/zh/processors/processor-numbers.html?_ga=1.52051882.281579711.1486467137)

<img src="{{ site.url }}/pic/{{page.title}}/cpuIdentity.jpg" />
<div class="text" style="text-align:center;font-weight:bold"> CPU型号</div>


<img src="{{ site.url }}/pic/{{page.title}}/cpuIdentitySuffix.jpg" />
<div class="text" style="text-align:center;font-weight:bold"> CPU型号后缀含义</div>

补充：[link](http://diy.pconline.com.cn/718/7189243_all.html)
- Desktop
  - X代表Extreme，中文意思是至尊级，代表同一时代性能最强的CPU。如Core i7-5960X、Core i7-4960X。X代表在同一代中只有一款CPU黄袍加身，地位至高无上。加上没有竞争对手可以望其项背，从露面都退出市场，期待的弑君者没有出现。
  - 自从Sandy Bridge时代Intel限制超频之后，K后缀成为了超频的标志。从i7-2600K开始到现在的i7-6700K，但凡带K后缀的CPU都解锁倍频，可自由调节。此外，K后缀还代表着同样数字型号的最高规格，比如i7-6700K的性能强于i7-6700。
  - T后缀的CPU在功耗上更加低，为45W或更低，频率也比S后缀的更低。比如2.5GHz-3.7GHz的i7-4770T（对比i7-4770K为3.4GHz-3.9GHz）。
  - R代表采用了Iris高性能核显（核心显卡）的R。Iris Pro代表了同类产品中核显最强的系列，比如i7-4770R、i7-5775R。不过这款产品的TDP为65W，为节能版。
  - 结论：大多数消费者只需要考虑无字母后缀的CPU就行，毕竟它们定位大众。**而追求性能或者超频发烧友就选择K后缀的高性能CPU**。至于其他后缀的，由于定位不符合大众化，所以我们就接触得少。
- 移动端
  - M代表了双核标准功耗版本的笔记本CPU。在四代或者以前的酷睿时代，性价比高，性能适中，让35W/37W的M后缀的CPU一直是主流笔记本的绝对主流，如大家耳熟能详的i5-4200M，i3-2310M等。可惜到了五代酷睿之后，M后缀的CPU成为了绝唱，我们再也找不到新一代标准功耗的双核CPU，i7-4610M成为了最后一款以单线程高频率为代表的高性能i7双核笔记本CPU。
  - 太多了，不写了。
