---
layout: post
title: 仅Docker还不够
modified:
categories: container
tags: [docker, paas]
comments: true
image:
  feature:
date: 2015-10-05T00:00:00+08:00
---
If you're here, you might know about docker, if you don't know about docker, please refer to the following articles, videos and slides about docker, which might capture your interest [0].

This article is not about docker, it's more about the Co., the eCosystem around docker and dockerization of your apps, services and last but not least about bringing the DevOps style and a new way of thinking to your business.

![](/images/2015/10/docker-is-not-enough.png)

The following list below provides an overview of the Co., the eCosystem around docker, which for sure is not complete and is intended only as a reference for my own observation and research about these systems and why the eCosystem around docker is embracing and integrating docker and how we can use and combine them together for different use cases.

So let's have a look on this short list:

    Ansible & Docker
    Amazon EC2 & Docker
    Apache Brooklyn & Docker
    Apache Hadoop & Docker
    Apache Storm & Docker
    AppScale & Docker
    Atomic Hosts & Docker
    Chef & Docker
    Clocker & Docker
    Cloud & Docker
    Cloud Foundry & Docker
    CloudStack & Docker
    CoreOS & Docker
    Deis & Docker
    Decker & Docker
    Docker & Docker
    Dokku & Docker
    Eucalyptus & Docker
    Flynn & Docker
    Google Compute Platform & Docker
    IBM Bluemix & Docker
    Kubernetes & Docker
    Mesos & Docker
    Mesosphere & Docker
    Microsoft Azure & Docker
    OpenCamp & Docker
    OpenShift & Docker
    OpenStack & Docker
    Panamax & Docker
    Puppet & Docker
    SaltStack & Docker
    Shipyard & Docker
    Stackato & Docker
    Tsuru & Docker
    VMware & Docker

As we can see, these eCosystems have Docker in common, but you might ask if and how all these implement the integration and enhance their own system with Docker and play with others and which of them might be the right solution for different use cases, i.e. to build (web-) apps, services or a whole cloud platform to provide for instance SaaS offerings or to help to achieve higher density in data centers and control the VM sprawl, etc..

Well, first I'm going to classify these eCosystems in the following categories:

    Big Data
    Cloud Platform
    IaaS
    Data Center Operating System
    Docker Operating System
    Docker Management
    PaaS
    Orchestration and Configuration Management Engines

which leads to the following table matrix:

![](/images/2015/10/docker_and_co.png)

One row, one column and one cell might capture your eye here and someone might argue that "Docker & Docker" doesn't make any sense and why I'm classifying docker as IaaS, PaaS, Docker OS and as an Orchestration and Configuration Management Engine all together, well if we agree with the fact that we can build our IaaS and PaaS with docker and we can leverage the docker engine to orchestrate itself and run docker containers in docker containers, then I guess you'll agree with this view.

The other interesting point is the PaaS column, which shows that most of these eCosystems are trying to address the PaaS challenge in different ways with docker. But as Ben Gloub, CEO of Docker Inc. states in [15] "a lot of people are questioning whether they really need a full PaaS to build a flexible app environment" and exactly here enterprises need to re-think thier strategies, whether they need to use Macro PaaSes such as Cloud Foundry or OpenShift, or should they build more efficient lightweight custom PaaS solutions with docker and the ecosystem arround it, we refer to such a custom made PaaS solution as OpenCamp, the Open Cloud Application Management Platform.

But what about the one cell, what is Mesos or Mesosphere and what is a "Data Center Operating System"? Well, that's the new way of thinking, which I'm going to address in my next blog post.

But for now, if you ask me which of these systems are my favorites, I'd say Docker followed by Mesosphere, OpenStack, Kubernetes, Panamax and Deis!

Last but not least, I'd like to invite you to have a look on why "Docker Is Not Enough".



[0] Docker related articles

[1] Introduction to Docker by Solomon Hykes, Docker Founder and CTO at Docker Inc.
https://www.youtube.com/watch?v=Q5POuMHxW-0

[2] What is Docker
https://www.docker.com/whatisdocker/

[3] WHAT IS DOCKER AND WHEN TO USE IT
http://www.centurylinklabs.com/what-is-docker-and-when-to-use-it/

[4] How Docker Was Born | Leanstack
http://blog.leanstack.io/how-docker-was-born

[5] How Docker Fits Into The Current DevOps Landscape
http://blog.leanstack.io/how-docker-fits-into-the-current-devops-landscape

[6] Crash Course: Next Generation Servers With Containers by Sébastien Han:
http://www.sebastien-han.fr/blog/2014/02/03/crash-course-next-generation-servers-with-containers/

[7] Docker drops LXC as default execution environment
http://www.infoq.com/news/2014/03/docker_0_9

[8] The key differentiators of Docker technology by Pethuru Raj
http://thoughtsoncloud.com/2014/09/docker-managing-excitement/

[9] Docker is the Heroku Killer
http://www.brightball.com/devops/docker-is-the-heroku-killer

[10] KVM and Docker LXC Benchmarking with OpenStack
http://bodenr.blogspot.de/2014/05/kvm-and-docker-lxc-benchmarking-with.html

[11] VMware buys into Docker containers
http://www.zdnet.com/vmware-buys-into-docker-containers-7000032947/

[12] 4 ways Docker fundamentally changes application development
http://www.infoworld.com/article/2607128/application-development/4-ways-docker-fundamentally-changes-application-development.html

[13] The Docker exploit and the security of containers
https://blog.xenproject.org/2014/06/23/the-docker-exploit-and-the-security-of-containers/

[14] OpenShift v3 Platform Combines Docker, Kubernetes, Atomic and More
https://www.openshift.com/blogs/openshift-v3-platform-combines-docker-kubernetes-atomic-and-more

[15] Simplifying and securing multicloud development

http://www.pwc.com/en_US/us/technology-forecast/2014/issue1/interviews/interview-ben-golub-docker.jhtml

[16]  OpenStack & Docker

https://wiki.openstack.org/wiki/Docker

[17] Docker Jump Start by Andreaw Odewahn

https://github.com/odewahn/docker-jumpstart-source

From : https://www.cloudssky.com/en/blog/Docker-Is-Not-Enough/
