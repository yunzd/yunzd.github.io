---
layout: post
title: Container技术栈概述
modified:
categories: container
tags: [docker]
comments: true
image:
  feature:

---
应用架构驱动着基础设施架构的发展。下一代应用架构聚焦于社交、移动、大数据、智能硬件，驱动应用向着更好的用户体验发展，也对传统的技术架构产生了冲击。

本文我们将看下新一代技术架构的元素，以及公司在下一代应用和基础设施中，应当如何进行架构选型。


在过去的6-12个月里，Container技术引起了企业的CIO、架构师和开发人员的极大兴趣。

   * Container技术引起CIO兴趣的原因在于，他们找到了向现代应用框架快速转型的方法，从而驱动业务的发展。
   * 对于架构师来说，Container技术增强了应用的可移植性，适用于混合云架构。
   * 而开发人员通过Container技术，可以快速地应用微服务架构，增强应用的可扩展性。

DockerCon 和 Red Hat Summit 中，都曾讨论过这一趋势。但是每种新技术，都会带来新的挑战和利弊权衡。本文将对container技术架构中的元素逐一进行分析。

下图为Docker技术人员 Patrick Chanezon 总结的Container技术栈：

![docker tech stack](/images/2015/09/docker-stack.png)

在深入了解Container技术架构之前，有必要先理解以下几个基本问题：
   * Container技术非常适用于分布式应用框架中，这意味着底层的Container基础架构必须是模块化和非常灵活的；
   * 以下所探讨的核心概念，很多来源于如今的虚拟化架构，比如服务发现（Service Discovery），集群管理（Cluster Management）和资源调度（Resource Scheduling）；
   * 围绕整个container技术架构的公司生态也在快速发展，多数情况下，他们都从单一的工具开始，慢慢发展为一个全栈服务。

## 深入Container技术架构

从单一的Container到服务于产品的架构，需要一系列的工具和配置。从这些工具和配置上，我们可以看出下一代基于Container的应用需要哪些组件：

首先，要进行以下声明：
  * 所有这些组件都不是硬件的，也不依赖于云服务；
  * 在实现时，可能会结合一些组件到一个软件中；
  * 并非所有组件都是必须的。

下面是对整个技术栈自下而上的分析。

### DevOps工具

叫“开发工具”和“运维工具”更加直观，他们运行在本地机器上，用来启动Container，虚拟机或初始化应用包。

    Hashicorp Vagrant，Docker Machine，VMware AppCatalyst, Shippable, Wercker


### 物理宿主机（或VM）

Container实际运行的机器，可能是一个本地的笔记本电脑，一个数据服务器或一个公有云实例。

    VMware ESX，Microsoft Hyper-V，KVM，LXD


### Container OS

负责和底层物理宿主机交互的操作系统（Linux或Windows），还负责提供所有container共享的OS级别的服务。

    CoreOS，RedHat Atomic Host，Rancher OS，Canonical Snappy，VMware Photo


### Container Runtime

创建、启动、停止、销毁Container的软件层。

    Docker，Rkt，Lattice


### Container

管理着应用和Container OS之间的交互。也有人将其类比为虚拟机，但是container并不包含一个完整的操作系统。


### Marketplace/镜像管理

储存和管理container镜像的服务器（自己搭建或采用云端的SaaS应用）。

    Docker Registry，CoreOS Registry


### 配置管理（Configuration Management）

管理和自动化配置文件和部署的工具。

    比如Ansible，Chef，Puppet，Docker Compose，Terraform


### Container Networking

SDN，管理container之间包路由的网络框架。

    Docker Networking，WeaveWorks，Flannel (CoreOS), Calico


### Container集群管理

帮助建立Container集群，形成服务，管理集群资源，保障可用性。

    Docker Swarm, Hashicorp Serf


### 服务发现

随着更多的元素/微服务 被加入到应用中，找到这些服务并确认其是否可用变得很困难。服务发现可以在其加入网络中时，管理广播和服务许可。

    Docker Swarm, Hashicorp Serf


### Container 调度

这项服务保障了container在宿主机中处于最高效的位置，容量规划，失败时重启container，需求变化时扩展应用等。

    Kubernetes，Mesos


### 应用调度

和container调度的功能类似，是一系列面向应用的服务，描述应用被部署时所需的资源，时间，依赖等。

    Marathon，Chronos，Hadoop，Spark


### 安全

在container架构中，同样需要所有的安全组件，从OS到认证服务，到数据管理。

    CoreOS Linux，Intel Clear Containers，Docker Notary，Hashicorp Vault, OpenSCAP, Twistlock, Scalock, Conjur, Lynis

### 日志管理

    Docker logs, logspout

### 监控/可视化

    Docker ps/top/stats, Docker stats API, sysdig, cAdvisor, Weave Scope


### Container实现——灵活性，交互性以及挑战

对于企业来说，Container技术的挑战之一在于，选择Container技术栈意味着舍弃传统的，由ISV提供的一站式“单块架构”，通常会令企业望而生畏。

第二个挑战在于，初创公司常常将个人项目作为工具，许多情况下，要么是某公司在主导这个项目，要么由一个开源社区主导。

第三个挑战在于许多工具是彼此协作和可替代的。许多早期用户会试用不同项目的组件，而不是由一个供应商或开源项目提供的技术栈。短期来讲，有助于早期采用者实现灵活性和技术的多样性。长期来看，不利于企业对稳定环境的需求，同时也会增加对新技术的学习成本。相关的公有云服务，比如Amazon Elastic Cloud Services和 Google Container Engine，可以帮助用户屏蔽技术上的复杂性。

## 总结

Container技术和架构产生和发展都很迅速。许多初创企业都在驱动新工具和框架的产生，能够将合适的架构组件组合起来的技术，将为企业带来很多优势。不管基于已有的基础架构还是公有云，随着更多的解决方案级别的产品被推向市场，Container技术的发展脉络也将逐步清晰。

本文译自wikibon文章《Evolving Container Architectures》，原文请点击 [阅读原文](http://wikibon.com/evolving-container-architectures/)
