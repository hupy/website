+++
title = "系统架构"
description = ""
weight = 2
type = "docs"
+++

### 系统架构
---
Choerodon采用Spring Cloud作为微服务架构，本文介绍Spring Cloud微服务架构的概念并描述了Spring Cloud的功能，然后介绍基于Spring Cloud的各个组件搭建Choerodon的微服务整体架构，并对总体架构进行了设计和说明。

将从如下几个方面介绍：

- [企业级系统服务架构演进](#企业级系统服务架构演进)
    - [单体应用](#单体应用)
    - [垂直应用](#垂直应用)
    - [SOA](#SOA)
    - [微服务架构](#微服务架构)
- [Choerodon 微服务架构](#Choerodon 微服务架构)
    - [服务网关](#服务网关)
    - [认证中心](#认证中心)
    - [注册中心](#注册中心)
    - [配置服务](#配置服务)
    - [消息服务](#消息服务)
    - [服务监控](#服务监控)
    - [调用链追踪](#调用链追踪)

### 企业级系统服务架构演进
---

#### 单体应用

在诞生之初始，应用与数据库是部署在同一台机器上，这时的用户量、数据量规模都比较小，这样的架构既简单实用、便于维护，成本又低，成为了这个时代的主流架构方式（这种架构仍然存在，在一些小型应用或者实验环境仍是主流，主要还是成本与方便性的原因）。

随着安全意识的提高及技术的发展，数据与应用分离的呼声渐高，原始单一应用逐步转化为WebServer＋DatabaseServer的模式。这一阶段的发展已经走到的尽头。

#### 垂直应用

当单一应用不能满足需求时（主要是响应速度与吞吐量），如何改变架构以应对实际问题呢？问题是推动技术发展的源动力，当问题出现时，总是会有响应的解决方案，垂直型应用架构在这种背景下出现了。

所谓垂直型应用架构，是指将一个大的单一应用拆分成若干个小的单一应用，这样每个应用的压力大约有原来的1/n（粗略估计值）。这种拆分方式为后来的技 术发展奠定了基础，每次架构的变化都与这次有着密不可分的关系——分治（将一个大的问题按一定业务规则分成若干个小的问题，逐个解决）。

#### SOA

当垂直应用越来越多，业务上跨应用的交互不可避免，这给垂直应用架构带来了很大的挑战，如何才能进行跨应用的交互呢？RPC的出现解决了这个问 题，RPC作为分布式架构的核心内容，在提高业务复用、业务整合、架构扩展等方面发挥着不可替代的作用。因为RPC的实现方式有很多种，单一语言、跨语言 都存在相应的开源产品（当然也可以自建），这为RPC的使用及推广奠定了良好的基础。

此时需要注意，服务一定是无状态的、接口稳定的。

在分布式架构中，将剥离出来的核心业务构成的服务层相对独立、稳定，以这些服务为基础进行不同形式的组合使用，使前端（view、消费者）能够快速适应业务发展、变化。这时的前端架构也逐渐被从整体架构中剥离并越来越受到重视（尤其是移动应用的快速发展）。

#### 微服务架构

分布式架构中的服务越来越多，导致交互越发复杂，不可避免会出现资源浪费的情况。如何才能更好的管理复杂的调用关系、提高资源利用率、对整个服务集群进行动态控制。服务治理被引入来解决上述问题。

如何在服务化架构中实现服务治理以及要对哪些内容进行治理，需要根据实际情况进行取舍，因为每个服务化架构中关注的内容可能都会有所不同。治理的实现方案也没有统一的方案、规范，在能满足自身需要的前提下，尽量避免过度管理、控制。下面是一些建议：

    1.服务注册与发现，能做到自动化最好，尽量少的人工干预会给后期运维带来非常大的好处；
    2.路由、负载可以通过类似LVS这样的工具或者编码实现软复杂各有好处；
    3.服务升、降级分自动与手动，根据实际访问情况进行动态调整；
    4.服务监测、依赖关系尽量做到自动化；
    5.监控、统计一定要自动化，并且能够进行细粒度分析；


![](/img/docs/concept/system-infrastructure-increment.png)


### Choerodon 微服务架构
---

Choerodon 是基于Spring Cloud构建的微服务系统。构件一套完整的微服务架构需要考虑许多问题，包括API Gateway、服务间调用、服务发现、服务容错、服务部署、数据调用等。基于Spring Cloud构建微服务架构可以通过自动配置和绑定Spring环境和其他Spring编程模型来实现微服务。采用Spring Boot应用程序提供的集成功能，通过几个简单的注释，开发人员可以快速配置和启用应用程序中的常见功能模块，并使用久经考验的Netflix组件构建大型分布式系统。 提供的微服务功能模块包括服务发现（Eureka），断路器（Hystrix），智能路由（Zuul）和客户端负载均衡（Ribbon）等。下图显示了采用Spring Cloud系列平台构建的微服务整体架构。

![](/img/docs/concept/choerodon-infrastructure.png)

#### 服务网关

服务网关是微服务架构中一个不可或缺的部分。通过服务网关统一向外系统提供REST API的过程中，除了具备服务路由、均衡负载功能之外，它还具备了权限控制等功能。Spring Cloud Netflix中的Zuul就担任了这样的一个角色，为微服务架构提供了前门保护的作用，同时将权限控制这些较重的非业务逻辑内容迁移到服务路由层面，使得服务集群主体能够具备更高的可复用性和可测试性。

#### 认证中心

在Spring Cloud需要使用OAUTH2来实现多个微服务的统一认证授权，通过向OAUTH服务发送某个类型的grant type进行集中认证和授权，从而获得access_token，而这个token是受其他微服务信任的，我们在后续的访问可以通过access_token来进行，从而实现了微服务的统一认证授权。

#### 注册中心

Spring Cloud 封装了 Netflix 公司开发的 Eureka 模块来实现服务注册和发现。Eureka 采用了 C-S 的设计架构。Eureka Server 作为服务注册功能的服务器，它是服务注册中心。而系统中的其他微服务，使用 Eureka 的客户端连接到 Eureka Server，并维持心跳连接。这样系统的维护人员就可以通过 Eureka Server 来监控系统中各个微服务是否正常运行。Spring Cloud 的一些其他模块（比如Zuul）就可以通过 Eureka Server 来发现系统中的其他微服务，并执行相关的逻辑。

Eureka由两个组件组成：Eureka服务器和Eureka客户端。Eureka服务器用作服务注册服务器。Eureka客户端是一个java客户端，用来简化与服务器的交互、作为轮询负载均衡器，并提供服务的故障切换支持。Netflix在其生产环境中使用的是另外的客户端，它提供基于流量、资源利用率以及出错状态的加权负载均衡。

#### 配置服务

Spring Cloud Config为服务端和客户端提供了分布式系统的外部化配置支持。配置服务中心采用Git的方式存储配置文件，因此我们很容易部署修改，有助于对环境配置进行版本管理。

#### 消息服务

Kafka是分布式发布-订阅消息系统，最初由LinkedIn公司开发，之后成为之后成为Apache基金会的一部分，由Scala和Java编写。Kafka是一种快速、可扩展的、设计内在就是分布式的，分区的和可复制的提交日志服务。

#### 服务监控

Hystrix-dashboard是一款针对Hystrix进行实时监控的工具，通过Hystrix Dashboard我们可以在直观地看到各Hystrix Command的请求响应时间, 请求成功率等数据。但是只使用Hystrix Dashboard的话, 你只能看到单个应用内的服务信息, 这明显不够. 我们需要一个工具能让我们汇总系统内多个服务的数据并显示到Hystrix Dashboard上, 这个工具就是Turbine.

#### 调用链追踪

Zipkin 是一款开源的分布式实时数据追踪系统（Distributed Tracking System），基于 Google Dapper 的论文设计而来，由 Twitter 公司开发贡献。其主要功能是聚集来自各个异构系统的实时监控数据，用来追踪微服务架构下的系统延时问题。


