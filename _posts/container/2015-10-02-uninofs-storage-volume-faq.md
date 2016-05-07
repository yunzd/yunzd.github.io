---
layout: post
title: FAQ： 关于 Union Filesystems, Storage 和 Volumes
modified:
categories: container
excerpt:
tags: [docker]
comments: true
author: martin_liu
image:
  feature:
date: 2015-10-02T00:00:00+08:00

---
## Docker image 是分层的意味着什么?

It literally means that a docker images is built on layers. Each layer represents a portion of the images file system that either adds to or replaces the layer below it. For instance you might start with a Debian image, and then add Emacs, and the Apache. Finally, when you instantiate a container a final read-write layer to capture any changes that are made to the container.

 
## Union file system 是什么?

A union file system is a file system that amalgamates a collection of different file systems and directories (called branches) into a single logical file system.

 
## UnionFS 是分布式文件系统么？

No, it is not. 

## Docker Storage Drivers 和 Volumes 是一回事么？

No, they are actually quite different.

A storage driver is how docker implements a particular union file system. Keeping with are “batteries included, but replaceable” philosophy, Docker supports a number of different union file systems. For instance, Ubuntu’s default storage driver is AUFS, where for Red Hat and Centos it’s Device Mapper.

A volume is subdirectory in a container’s file system that exists outside of the union file system.  Unless you explicitly commit changes made to a Docker container during run time to a new image, they are lost when the container is destroyed. Volumes, by contrast, will not be automatically deleted when a container is removed (you can, of course, tell Docker to remove any volumes when removing a container).

## 用NFS共享卷作为容器的卷有什么问题么？

Not really. This is a pretty common scenario. 

## 如何将更新后的Docker容器导出为可以重用的新的Image？

You can do this by using docker commit. In an example where you have a container named “mywebsite” the following command would create a new image named “mikegcoleman\mynginx” from that container

~~~ bash
docker commit mywebsite mikegcoleman\mynginx
~~~
 

## 如何对容器做安全加固？

Visit the Docker Security resource center to get access to tools and best practices in securing your Docker implementation. 

## 如何配置容器对(RAM 和 CPU)资源的用量？

CPU and RAM can be configured via the docker run command. You can read about memory here, and CPU here.

 

## 我想用 RHEL based image 怎么办？

Red Hat hosts a variety of docker images, but you must be a Red Hat subscriber to access them.

 
## RHEL containers vs. Docker containers 他们的区别？

The Docker engine that runs on Red Hat Enterprise Linux (RHEL), offers the same functionality as the Docker engine running on any other Linux distribution. Additionally, If you are a Red Hat subscriber, you can run RHEL inside of a container on any host running Docker engine.

 

## 如果，我用 Solaris OS 运行 Docker 是支持的, 这意味这我能用来跑CentOS的容器么？

We are working with Oracle to implement Docker with [Solaris Zones](https://blog.docker.com/2015/08/docker-oracle-solaris-zones/), but this funcitonaity is not yet generally available. When it does ship, however, it will allow Docker to run Solaris workloads on Solaris, but it won’t allow to run Linux workloads on Solaris. Similarly, the Docker Engine that will run on Windows when Microsoft ships its container support with Windows Server 2016 will run Windows workloads, not Linux workloads.

That being said, the Illumos kernel has support for a feature called “BrandZ,” which allows running Linux binaries on the Illumos kernel. Joyent’s Triton product leverages on this feature to run Linux containers on their SmartDataCenter platform, which uses the Illumos kernel.


## 少数象 Oracle 12g RAC 这样的应用需要动态的调整 内核参数，而且有 fencing 的概念，Docker将会支持么？

It might work, but it’s not something we advocate as  a good use case. Docker is designed for microservices or distributed applications. Each container should, ideally, perform a single function. You tie those containers together to create complex applications. Large monolithic applications like Oracle, are not a particularly good use case for containers.

 

## 当运行10个Web容器，如何为他们设置共享文件系统数据，例如上传的文件？ 能用类似 GlusterFS 的文件系统么？

Without knowing more specifics on the use case, it’s hard to say exactly what the best approach would be. However, using a shared data-only container with volumes mounted to house the shared data would be a good place to start.

As for Gluster, I’m not sure it’ll solve the problem you’re trying to solve by itself. That being said, customers have deployed Docker volumes to GlusterFS with success. Just make sure you understand the characteristics of the file system – in particular how to tune it appropriately.

 

## 如果你去配置和映射一个本地目录作为容器的一个卷，当你需要让这个应用上生产环境，最佳实践是什么？

The app code should be copied into the Docker image. The idea is that your Docker image includes everything the app needs to execute including the code and any libraries or dependencies.

 

## 由于 data-only containers 依然存储数据在运行的主机上，为什么这还是会比 host-mounting 方式要好？

Host mounting  ties the volume mount point to one specific Docker host. With data-only containers, the volume is mounted to a container, and that abstraction works across different Docker hosts.

Wouldn’t a data container suffer from the same problem as writing data to a container over time? I.e. the build up of diffs may cause longer look up times due to folder traversal.

When we talk about data-only containers, we talk about them in the context of hosting a volume in that container. Since we’re using a volume, it bypasses the union file system, and isn’t affected by some of the performance issues.

 

## 备份和版本控制的最佳实践是什么？

These are pretty broad topics, but I’ll give a couple quick answers with the fact that we’re doing a session on October 27th that will talk about backups in pretty good detail.

Part of the reason for storing your data in volumes is it makes it fairly trivial to backup. Information on the hows and whats can be found in the Docker documentation.

As for version control, I’m not exactly sure what you mean, but I’ll take a crack at it anyway. As a best practice we recommend using Dockerfiles to control the creation of new Docker images. Those Dockerfiles should be stored in GitHub, and treated like any other source code. Additionally, when images are pushed to a registry, they can be tagged with a version number so you can keep older versions around or be explicit about which version you wish to use in the future. 

来源：[http://blog.docker.com/2015/10/docker-basics-webinar-qa/](http://blog.docker.com/2015/10/docker-basics-webinar-qa/)
