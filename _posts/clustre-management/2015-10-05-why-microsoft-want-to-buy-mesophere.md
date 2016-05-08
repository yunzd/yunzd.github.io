---
layout: post
title: 为什么微软欲10亿收购Mesosphere
modified:
categories: cluster-management
tags: [docker, microsoft, mesosphere]
comments: true
show_meta: true

image:
  feature:
date: 2015-10-05T00:00:00+08:00
---

具报道微软正在和一家快速增长的云计算创业公司 [Mesosphere](https://mesosphere.com/) 商讨并购事宜。根据湾区行业媒体 “The Information” 记者 Amir Efrati 的报道，这家创业公司目前估值接近10亿美元。

交易的细节目前还不清楚 —— 记者没有说明交易进行到哪个阶段，也没有说明微软内部对于这个估值的态度。 Mesosphere 和微软都对这个消息表示无可奉告。

让微软开出如此天价的除了 Mesosphere 的技术，最近硅谷对于软件定义的数据中心的热情肯定也是个因素。数十亿美元的热钱涌入这个领域，创业公司门前排满了传统 IT 巨头，谁也不想在制定行业标准和抢占行业制高点方面丢了先机。到底 Mesosphere 到底是何方神圣，为何能十亿级别估值？

## Mesosphere 这个公司是干啥的？

作为一家飞速增长的创业公司，自 2013 年来 Mesosphere 专注于围绕开源软件 Apache Mesos 开发产品和服务。Twitter 这样的巨头为了应对集群的飞速膨胀，借助 Mesos 将数以千计的服务器整合地像使用一台电脑，同时还管理运行这台“电脑”中成千上万的任务。 另外一家独角兽公司 Docker，则支持将他们火爆的容器产品部署到 Mesos 集群中。Docker 依靠其简单灵活的容器产品一夜成名，开发者终于能够在统一的环境中开发、测试和发布应用，将过去数周的工作缩减到几个小时。Yelp 这样的巨头就将 Mesos 和 Docker 组合起来，使用 Mesos 将整个数据中心变成一个资源池，然后通过 Docker 将应用部署到整个集群。

类似其他提供基础设施和数据的管理软件的创业公司，Mesosphere 也来自真正拥有“大数据”的门派。CEO Florian Leibert 在独立创业之前，曾经是 Airbnb 和 Twitter 的技术负责人（就连巨头 Google 内部也有一个功能类似 Mesos 的管理平台 Borg）。Leibert 将 Mesos 引入了 Airbnb，这样技术团队可以轻松地在其上搭建 Spark 大数据运算系统。

今年春天在一次福布斯杂志采访中，Leibert 告诉记者他们的产品能让数据中心非常容易管理，不论里面有五台还是五千台机器。一个佐证是掌管着 Twitter 数千台服务器仅仅是屈指可数的几位运维工程师。

说起他们创业最苦逼的事情，Leibert 苦笑道 “怎么和大家说明白我们是干啥的”。最后总结成一句话，让你们服务器的利用率提高两三倍。

## 微软 看上她哪点了？

从微软的角度来说，通过并购获得 Mesos 技术，就能够迅速在这一波软件定义的基础设施大潮中站稳脚跟。之前 LinkedIn 已经天使投资了另外一家相同领域的创业公司 Confluent，Google 和 IBM 则联合 Docker、Mesosphere 一起来制定新的容器标准（Open Container Project），微软至今毫无建树。正如 Efrati 在他的访谈中提到，虽然微软已经参与了 Docker 项目，但在上述大领域中还没有能占一席之地的项目。

大家不要忘记，六月份的时候坊间曾经流传微软要并购 Docker。根据微软的风格，在做出并购决定之前，还会仔细评估这些创业公司一阵。

2014年底，Mesosphere 得到了 Khosla Venture 的3600万美元B轮融资，之前A轮则是由 Andreessen Horowitz 领投。根据 Pitchbook 的数据，Mesosphere 的 A 轮估值约为 4500 万美元，B 轮之后增加到数亿美元。同期 Docker 公司经过 C 轮融资估值达到 4 亿美元，随后四月份更是加入了独角兽俱乐部（十亿美元）。

虽然我们现在还不确定未来 Docker 被巨头并购时的估值，但是 Mesospher 如果能够比 Docker 成长地更快，那么虽然盈利不大，但足以让它的估值从一亿美元猛增到十亿美元（在新兴产业领域，人们对增长速度的关注远远高于现实盈利）。如果最终证实了 The Information 报道的估值，也算是个合情合理的价格。

大家都拭目以待的是微软是否真的下定决心在这个新兴领域放手一搏，就算竞争对手已经找好了同盟军。当然大家也关心 Mesosphere 是否现在就想卖，即使这个估值听起来已经不太小。

来自：2015-08-19 福布斯记者 Alex Konrad
http://www.forbes.com/sites/alexkonrad/2015/08/18/why-microsoft-could-reportedly-want-to-buy-cloud-startup-mesosphere-even-at-1-billion/#rd
