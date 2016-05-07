---
layout: post
title: Docker 1.8发布带来的新工具
modified:
categories: container-runtime
excerpt:
tags: [docker]
comments: true
image:
  feature:
date: 2015-09-27T00:00:00+08:00
---

Docker公司宣布Docker 1.8发布，该版本包括工具的新增和更新，同时带来了新的引擎特性。Docker工具箱（Docker Toolbox）提供了打包的系统，目标是成为“获取和使用Docker开发环境运行的最快方式”，并替换Boot2Docker。Docker内容信任（Docker Content Trust）是Docker引擎最重大的变化，它提供了镜像签名和验证。

Docker工具箱为Windows和Mac用户捆绑了这些组件：

* VirtualBox：运行包含Docker引擎的轻量Linux虚拟机
* Docker Machine：一个配置工具，用于将Docker安装到VirtualBox（同样可以将Docker安装到多个云服务上）。
* 操作系统使用的Docker客户端
* Kitematic：Docker图形用户界面
* Mac版本包括Docker Compose合成工具（来自他们为Fig收购的Orchard Labs），帮助多容器应用配置。

安装指南为使用Boot2Docker或者已经安装VirtualBox的用户提供了迁移指导。在Windows和Mac系统上使用其他虚拟化环境的，如Hyper-V、Parallels、VMware工作站或Fusion平台，没有包括在迁移指导内（和这些平台的潜在冲突也没有提及）。Windows 10暂时还不支持。

Docker内容信任基于Notary项目，它是更新框架（The Update Framework，TUF）的一种“自以为是的实现”，是一个“可以添加到软件更新器上的弹性安全框架”。它通过使用签名，为在不安全介质上分发镜像提供安全保障。它同样提供了新鲜度保证，因此当有人在下载一个镜像时，能够知道不会获取到可能包含隐患的旧版本。这套系统使用了三种密钥类型：用于离线保存的根内容信任密钥、每个镜像仓库独立的标签密钥和时间戳密钥。当内容信任开启时（目前由环境变量控制），离线密钥和标签密钥在镜像第一次推送是自动生成。该系统还没有完全集成到Docker Hub服务上，因此自动构建无法签名。Docker Hub中的库镜像（如Ubuntu）目前已经签名，对推送到Docker Hub的用户镜像也可以进行签名了。Docker公司的David Lawrence提供一个在线聚会上录制的演示。

![docker Notary](/images/2015/09/1ContentTrustKeys.jpg)

Docker引擎更新后，将对卷插件的支持从“实验性”改成了“稳定”，表明存储能够和生态系统提供者提供的存储介质集成，如Blockbridge、Ceph、ClusterHQ、EMC和Portworx。日志驱动增加了一些特性，包括对Graylog扩展日志格式(Graylog Extended Log Format，GELF)支持、Fluentd和syslogd结合（从1.6版本开始支持）。由于有推送镜像相关缺陷，Docker 1.8.1已经发布，用于替换Docker 1.8.0。

由于docker二进制文件同时充当了客户端和后台，一些努力已经花费在在区分这两个角色和明确各自文档上。最显著的区别是“-d”命令行开关已经被废弃，转而使用“daemon”参数，后者也有了自己的帮助文档。目前也支持通过“docker cp”命令从宿主机向容器复制文件（之前版本已经支持从容器向宿主机复制文件）。

对于Linux用户，Debian和Ubuntu发行版有了新的apt仓库，RHEL、CentOS和Fedora有了新的yum仓库。两个仓库中的安装包，都从“lxc-docker”重命名称了“docker-engine”，主要考虑到Docker已经抛弃LXC作为默认执行驱动16个月，避免因此产生迷惑。切换到新的仓库需要特别小心，因为一些Docker配置文件中的自定义内容可能会丢失，且需要列出当前运行的容器时“docker”命令可能会不存在。

Docker的流程控制工具也已经更新，Compose升级到1.4版本，Machine和Swarm发布了0.4版本。这些工具新版本有许多功能提升，包括一些为了工具箱实现的功能，其中一些功能将会吸引那些通过类似Mesos将Docker运行在大规模集群中的用户。2.1版本的Docker注册中心已经发布，提供了更快的操作和一些新特性。

此次发布的重要主题是安全和易用性。前者主要针对大企业用户，后者主要目的是让更多开发者上手Docker。Docker将会找到自己的方式运行在Windows Server 2016上，对此现在已经有技术预览版。因此Docker公司无疑将会把目光投向Windows用户群。

转帖：http://www.infoq.com/cn/news/2015/09/Docker-1-8
原文：http://www.infoq.com/news/2015/08/Docker-1-8
