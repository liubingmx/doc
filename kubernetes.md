### kubernetes 概述
- 容器技术概述
 1. [容器简史](https://www.cnblogs.com/bakari/p/8868850.html)

>容器概念始于 1979 年提出的 UNIX chroot，它是一个 UNIX 操作系统的系统调用，将一个进程及其子进程的根目录改变到文件系统中的一个新位置，让这些进程只能访问到这个新的位置，从而达到了进程隔离的目的。
2000 年的时候 FreeBSD 开发了一个类似于 chroot 的容器技术 Jails，这是最早期，也是功能最多的容器技术。Jails 英译过来是监狱的意思，这个“监狱”（用沙盒更为准确）包含了文件系统、用户、网络、进程等的隔离。

>2001 Linux 也发布自己的容器技术 Linux VServer，2004 Solaris 也发布了 Solaris Containers，两者都将资源进行划分，形成一个个 zones，又叫做虚拟服务器。

>2005 年推出 OpenVZ，它通过对 Linux 内核进行补丁来提供虚拟化的支持，每个 OpenVZ 容器完整支持了文件系统、用户及用户组、进程、网络、设备和 IPC 对象的隔离。

>2007 年 Google 实现了 Control Groups( cgroups )，并加入到 Linux 内核中，这是划时代的，为后期容器的资源配额提供了技术保障。

>2008 年基于 cgroups 和 linux namespace 推出了第一个最为完善的 Linux 容器 LXC。

>2013 年推出到现在为止最为流行和使用最广泛的容器 Docker，相比其他早期的容器技术，Docker 引入了一整套容器管理的生态系统，包括分层的镜像模型，容器注册库，友好的 Rest API。

>2014 年 CoreOS 也推出了一个类似于 Docker 的容器 Rocket，CoreOS 一个更加轻量级的 Linux 操作系统，在安全性上比 Docker 更严格。

>2016 年微软也在 Windows 上提供了容器的支持，Docker 可以以原生方式运行在 Windows 上，而不是需要使用 Linux 虚拟机。

基本上到这个时间节点，容器技术就已经很成熟了，再往后就是容器云的发展，由此也衍生出多种容器云的平台管理技术，其中以 kubernetes 最为出众，有了这样一些细粒度的容器集群管理技术，也为微服务的发展奠定了基石。因此，对于未来来说，应用的微服务化是一个较大的趋势。

 2. docker

>Docker 最初是 dotCloud 公司创始人 Solomon Hykes 在法国期间发起的一个公司内部项目，它是基于 dotCloud 公司多年云服务技术的一次革新，并于
2013 年 3 月以 Apache 2.0 授权协议开源，主要项目代码在
GitHub上进行维护。Docker 项目后来还加入了 Linux 基金会，并成立推动
开放容器联盟（OCI）。 

>Docker 自开源后受到广泛的关注和讨论，至今其 GitHub 项目已经超过 4 万 6 千个星标和一万多个 fork。甚至由于 Docker 项目的火爆，在 2013 年底，dotCloud 公司决定改名为 Docker。Docker 最初是在 Ubuntu 12.04 上开发实现的；Red Hat 则从 RHEL 6.5 开始对 Docker 进行支持；Google 也在其 PaaS 产品中广泛应用 Docker。 

>Docker 使用 Google 公司推出的Go 语言进行开发实现，基于 Linux 内核的
cgroup，namespace，以及AUFS类的Union FS等技术，对进程进行封装隔离，属于操作系统层面的虚拟化技术。由于隔离的进程独立于宿主和其它的隔离的进程，因此也称其为容器。最初实现是基于LXC，从 0.7 版本以后开始去除 LXC，转而使用自行开发的libcontainer，从 1.11 开始，则进一步演进为使用runC和containerd。
Docker 在容器的基础上，进行了进一步的封装，从文件系统、网络互联到进程隔离等等，极大的简化了容器的创建和维护。使得 Docker 技术比虚拟机技术更为轻便、快捷。 

>Docker 和传统虚拟化方式的不同之处。传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程；而容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，而且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便。

- docker解决的问题
   1. Docker解决了运行环境和配置问题，方便发布，也就方便做持续集成。
      
   2. 更轻量的虚拟化，节省了虚拟机的性能损耗
- docker的限制
   docker 非常适合用于管理单个容器，但是事实上随着业务的复杂度、我们的服务会变的越来越多，容器也会越来越多，就会导致一些问题——编排、管理和调度等各个方面，都不容易。于是，人们迫切需要一套管理系统，对Docker及容器进行更高级更灵活的管理。这时候以kubernetes为代表的基于容器的集群管理平台出现了。
   
- kubernetes概述
 1. kubernetes 简史
    K8S是2014年6月由Google公司正式公布出来并宣布开源的，并于2015年7月21日正式发布 Kubernetes v1.0 版本。它的前身，是Google自己捣鼓了十多年的Borg系统。 Borg是Google公司内部使用的大规模集群管理系统，它构建于容器技术之上，目的是实现资源管理的自动化，以及跨多个数据中心的资源利用率最大化。
 2. kubernetes 特性
 3. kubernetes 概念和术语
 4. kubernetes 架构及核心组件 
 
### kubadm一键部署利器（芬兰高中生通过业余时间完成的， 厉害了）
### pod控制器
  1. demonset
  2. replicaset
  3. deployment
  4. job
  5. cornjob
### service 和Ingress
   集群内pod的访问入口，
 1. 服务发现
    - coredns 
 2. 服务暴露
 
### 存储卷和数据持久化

### 配置configmap和secret
- 可以通过命令行配置
- 可以通过yaml文件进行配置
- 敏感信息secret
### 认证授权 rbac
### 网络模型
### Helm 程序包管理器

从开发到测试到运维大家所需要做的工作，怎么配合？
