---
layout:     post
title:      "北京家庭摇号决策"
date:       2023-05-27 06:52:00
author:     "Soldier828"
header-img: "img/post-bg-universe.jpg"
catalog: true
tags:
    - topic search
---

> 长久以来，不是很确切知道切成家庭摇号会是多少分

# 数据分析

> update 2023.06  家庭最低分60

家庭配置结果公布，根据pdf整理为excel

[2023家庭配置结果整理]({{ site.url }}/img/car_lottery/family_car2023.xlsx)

<img src="{{ site.url }}/img/car_lottery/data_analyse.png" style="border: 2px outset gray;">

### 历史数据汇总

- 家庭最低分
  - 2023年，家庭最低分60
    - 门槛处主要为两代群体。看着像存量自然增长，两代过了1年，增加2*2=4
    - 60,62,63,64合计1w个家庭，其中70%是二代。
    - \>=66分有4w家庭，位于p20位置。有娃现状+以后年度自然增长，相对安全。
  - 2022年，家庭最低分56

- 指标数量
  - 2023年小客车指标配额为10万个。普通指标额度3万个，新能源指标额度7万个。
    - 普通小客车指标，家庭和个人指标额度共计28600个, 家庭和个人同池摇号；单位指标额度1200个；营运小客车指标额度200个。
    - 新能源小客车指标。家庭和个人指标额度共计63600个，其中家庭指标额度50880个、占比80%，个人指标额度12720个、占比20%；单位指标额度3600个；营运指标额度2800个。
    - **加大新能源家庭比例到80%**
  - 2022年小客车指标年度配额为10万个。普通指标额度3万个，新能源指标额度7万个。
    - 普通指标，家庭和个人指标额度共计28600个, 家庭和个人同池摇号。
    - 新能源，家庭和个人指标额度共计63600个，其中家庭指标额度44520个、占比70%，个人指标额度19080个、占比30%
    - **加大新能源比例到70%**
    - **加大新能源家庭比例到70%**
  - 2021年小客车指标年度配额为10万个。普通指标额度4万个，新能源指标额度6万个
    - 普通指标，个人和家庭指标额度占年度普通小客车指标配额的95.5%，共计38200个，个人和家庭同池摇号
    - 新能源，家庭指标额度占年度新能源小客车指标配额约54.2%，共计32520个，个人指标额度占年度指标配额约36.1%，共计21680个。截至21年3月8日，共有153944个家庭申请了新能源小客车指标，403129名个人申请了新能源小客车指标
    - **首次家庭粒度**
    - **新能源家庭比例54%**
    - 一次性首批家庭摇号，北京市面向“无车家庭”一次性增发2万个新能源小客车指标
  - 20年公布数据（10万个）普通指标额度4万个。新能源指标额度6万个。
    - 个人指标额度占年度指标配额约90.3%，共计54200个；
  - 19年公布数据（10万个）普通指标额度4万个。新能源指标额度6万个。
    - 个人指标额度占年度指标配额的90%，共计54000个；
  - 18年公布数据（10万个）普通指标额度4万个。新能源指标额度6万个。
    - 个人指标额度占年度指标配额的90%，共计54000个；
    - **15w减少到10w**，新能源不变，油车变少。
  - 17年公布数据（15万个）普通指标额度9万个。示范应用新能源指标额度6万个。
    - 个人指标额度占年度指标配额的85%，共计51000个
  - 16年公布数据（15万个）普通指标额度9万个。示范应用新能源指标额度6万个。
    - 个人指标额度占年度指标配额的85%

# 政策解读

> 任何时候都要找官方doc，当下搜索引擎的top1满足率太低

官方文档:[申请小客车指标办事说明（家庭）](https://xkczb.jtw.beijing.gov.cn/bszn/20201230/1609342087846_1.html)

### 核心规则
- `家庭申请人积分=家庭申请人基础积分+家庭申请人阶梯（轮候）积分`
  - 基础积分。家庭**主**申请人的基础积分为2分，其他家庭申请人的基础积分为每人1分。
  - 阶梯（轮候）积分。
    - 油车摇号。按其累积的阶梯数每1阶梯加1分。具体规则如下图。
    - 电车排队。按其最近一次**开始轮候**的时间距离家庭摇号申请年上一年12月31日，每满一年加1分。
    - 以往油车摇号，目前电车排队，则两者分数相加。
- `总积分=[(主申请人积分+配偶积分)×2+其他成员积分之和]×家庭代际数`
  - 每一个家庭申请人(如子女、父母)，如果没有参与过摇号，即为基础积分1分
- 以家庭为单位申请每满一年，所有家庭申请人积分各增加1分。

<img src="{{ site.url }}/img/car_lottery/gas_car_score.png" style="border: 2px outset gray;">

### 网络demo

<img src="{{ site.url }}/img/car_lottery/car_score_demo.png" style="border: 2px outset gray;">

### 我们多少分

- 男方情况如下图
`score=2(主申请)+4(老油车)+2(新油车)=8`
<img src="{{ site.url }}/img/car_lottery/husband_score.png" style="border: 2px outset gray;">

- 女方情况如下图
`score=1+3(老油车)+4(18.09~22.12.31)=8`
<img src="{{ site.url }}/img/car_lottery/wife_score.png" style="border: 2px outset gray;">

> update: 2023.06

现状
`家庭总分=(8+8)*2=32`

有一个孩子
`家庭总分=[(8+8)*2+1]*2=66`

父母随迁
`家庭总分=[(8+8)*2+2]*2=68`


# 后续摇号方案决策
维持现状
<img src="{{ site.url }}/img/car_lottery/lottery_decision.png" style="border: 2px outset gray;">