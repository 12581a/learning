# SpringCloud

## 微服务概述



## SpringCloud概述

![](D:\TyporaFile\图片\20200325151153721.png)

1.常用组件

*  **服务发现——Netflix Eureka**

Spring-Cloud Euraka是Spring Cloud集合中一个组件，它是对Euraka的集成，用于服务注册和发现。 

Eureka由多个instance(服务实例)组成，这些服务实例可以分为两种：Eureka Server和Eureka Client。为了便于理解，我们将Eureka client再分为Service Provider和Service Consumer。

Eureka Server 提供服务注册和发现

Service Provider 服务提供方，将自身服务注册到Eureka，从而使服务消费方能够找到

Service Consumer服务消费方，从Eureka获取注册服务列表，从而能够消费服务

自我保护模式，是Eureka 提供的一个特性，在默认的情况下，这个属性是打开的，而且也建议线上都使用这个特性。如果Eureka Server在一定时间内没有接收到某个微服务实例的心跳，Eureka Server将会注销该实例（默认90秒）。但是当网络分区故障发生时，微服务与Eureka Server之间无法正常通信，此时会触发Eureka Server进入保护模式，进入自我保护模式后，将会保护服务注册表中的信息，不再删除服务注册表中的数据。

* **客服端负载均衡——Netflix Ribbon**

负载均衡简单的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的HA (高用)。

* **断路器——Netflix Hystrix**



* **服务网关——Netflix Zuul**



* **分布式配置——Spring Cloud Config** 









































