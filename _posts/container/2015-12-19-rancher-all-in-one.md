---
layout: post
title: 为容器而定制的超融合基础架构
modified:
description: 超融合基础架构是这种复杂性的优雅的解决方案，这是一种简便的开机即用的基础架构消费使用体验。超融合基础架构吧底层的复杂性隐藏起来，并使数据中心的操作人员运维起来更加容易。
categories: container
tags: [docker, rancher]
image:
  feature:
  credit:
  creditlink:
comments: true
share: true
---
转载自：[Rancher Blog](http://rancher.com/introducing-hyper-converged-infrastructure-for-containers-powered-by-rancher-and-redapt/)
原文作者 Liang Sheng


近几年在数据中心里，超融合基础架构是最具创新的技术之一。从我听Nutanix要做“数据中心的iPhone”的比喻以后，我就成了超融合基础架构的铁粉，这家公司是这项技术的创造者。我以前经理过很多职务：Cloud.com的CEO，CloudStack创始人和Citrix公司CloudPlatform业务部门CTO，我帮助了很多想把数据中心转型到下一代云基础架构的公司和企业。最大的挑战一直是如何协调有序地把来自各种厂商的不同的技术集成到一个可靠的云平台中。超融合基础架构是这种复杂性的优雅的解决方案，这是一种简便的开机即用的基础架构消费使用体验。超融合基础架构把底层的复杂性隐藏了起来，并使数据中心的操作人员运维起来更加容易。

![逻辑架构图](/images/2015/12/converged-nodes-300x122.png)

典型的，超融合基础架构被用来允许虚拟机，虚拟机是当今数据中心里普遍的工作负载。然而，数据中心工作负载的类型也正在发生着变化。在去年，Docker 容器已经成为一种颇受关注的工作负载类型。因此，我们开始看到市场对为容器而定制和优化的基础架构解决方案的需求。

今天，[我们Rancher团队和Redapt联合发布了容器超融合基础架构平台](http://www.businesswire.com/news/home/20151111006591/en/Rancher-Labs-Redapt-Introduce-Hyper-Converged-Infrastructure-Platform)。这个解决方案可以在数据中心里一键式地开启一套完整的容器服务平台，并集成了容器的自动化配置部署的工具。

## 支持虚拟机和容器

我们的解决方案设计为同时支持虚拟机和容器，这种实现方式是被 [Google使用且验证过的](http://www.enterprisetech.com/2014/05/28/google-runs-software-containers/)。从今年四月开始，我们在 Rahcher VM 项目中已经尝试了这个方案，并且从很多用户那边得到了非常正面的反馈。在容器中运行虚拟机的好处是可以用相同的工具来同时管理虚拟机和容器。由于虚拟机和容器在事实上行为方式类似，我们为 Docker 容器所开发的 Rancher CLI 命令行和 UI 图形界面可以无缝地适用于虚拟机。

我们使用 Rancher OS 做为融合基础架构平台的基础操作系统。Rancher OS 的内核被编译成可以支持 KVM 虚拟化。下图描绘了 Rancher 和 Rancher OS 是如何共同构成了我们的超融合基础架构解决方案平台的完整软件堆栈。

![Rancher Coverged Infrastructure](/images/2015/12/graphicv4_redaptv2.png)

## 容器化的存储服务

所有超融合基础架构方案包含一个分布式存储的组件。通过利用到 [我们今天的另外一项重大发布——持久存储服务](http://rancher.com/how-rancher-storage-services-unleash-the-power-of-software-defined-storage/)，这个超融合基础架构具备了独特的使用多种分布式存储的能力。用户可以自行决定部署适合他们应用的软件定义存储平台。这种方案不仅缩小了故障范围，且提高了可靠性。某个分布式存储后台的故障能造成的影响仅仅是所消费使用它的应用。

用户能同时部署开源和商业的存储软件，只要这些存储软件能像 Docker 容器一样的打包。我们正在把 Gluster 和 Nexenta Edge 纳入到我们的超融合基础架构平台中，并在未来计划支持更多的存储产品。

![Storage Services](/images/2015/12/converged-infrastructure-for-containers.png)

## 引入Docker 镜像生态系统

成功的超融合基础架构方案，通常会以运行流行的应用工作负载为目标，如数据库或者虚拟桌面。Docker 生态系统为 Rancher 超融合架构方案提供了可以运行的丰富的应用。仅拿 Docker Hub 举例，它提供了成千上万的 Docker 镜像。另外，Rancher 不仅让运行单个容器变得很容易，它更可以通过 Compose、Swarm 和 Kubernets 来调度和部署大规模的应用群集。Rancher Labs 完成了一组流行的DevOps工具的认证和打包。用户可以一键式地部署一个完整的ELK群集到超融合架构上。
![应用商店](/images/2015/12/catalog1-1024x438.png)

## 我们与Redapt成为合作伙伴

![Redapt logo](/images/2015/12/redaptlogo-300x101.png)

我们与Redapt已经认识并且工作在一起已经多年了。早在2011年，我在 Cloud.com 的团队与Redapt合作，用CloudStack共同搭建了当时最大的私有云，由超过40000台物理服务器构成。我们对他们的技术能力、创新能力和专业性感到非常钦佩。构建一个超融合基础架构方案需求软件厂商和硬件厂商之间的密切协作。我们很幸运地再次与Redapt携手，共同为市场打造专门为容器方案定制的首款超融合基础架构。
