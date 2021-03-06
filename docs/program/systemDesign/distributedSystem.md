# 分布式系统

## 分布式系统的问题

通信异常：网络本身不可靠，即使分布式系统各节点之间的网络通信能够正常执行，其延时也会大于单机操作，存在巨大的延时差别，也会影响消息的收发过程，因此消息丢失和消息延迟变的非常普遍。

带来了系统的复杂性，如分布式事务、分布式锁、分布式session、数据一致性 

## 分布式理论：一致性

分布式数据一致性，指的是数据在多份副本中存储时，各副本中的数据是一致的。 

副本一致性：

分布式系统当中，数据往往会有多个**副本**。如果是一台数据库处理所有的数据请求，那么通过ACID四原则，基本 可以保证数据的一致性。而多个副本就需要保证数据会有多份拷贝。这就带来了同步的问题，因为我们几乎没有办 法保证可以同时更新所有机器当中的包括备份所有数据。 网络延迟，即使我在同一时间给所有机器发送了更新数据 的请求，也不能保证这些请求被响应的时间保持一致存在时间差，就会存在某些机器之间的数据不一致的情况。

一致性分类：

**强一致性**：这种一致性级别是最符合用户直觉的，它要求系统写入什么，读出来的也会是什么，用户体验好，但实现起来往往 对系统的性能影响大。但是强一致性很难实现。

**弱一致性**
这种一致性级别约束了系统在写入成功后，不承诺立即可以读到写入的值，也不承诺多久之后数据能够达到一致， 但会尽可能地保证到某个时间级别（比如秒级别）后，数据能够达到一致状态。

## CAP定理

CAP 理论含义是，一个分布式系统不可能同时满足一致性（C:Consistency)，可用性（A: Availability）和分区容错 性（P：Partition tolerance）这三个基本需求，最多只能同时满足其中的2个。

| 选项         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| C 一致性     | 分布式系统当中的一致性指的是所有节点的数据一致，或者说是所有副本的数据一致 |
| A 可用性     | 也就是说系统一直可用，而且服务一直保持正常                   |
| P 分区容错性 | 系统在遇到一些节点或者网络分区故障的时候，仍然能够提供满足一致性和可用性的服务 |

## BASE理论

什么是BASE理论 :

- Basically Available(基本可用)

 基本可用是指分布式系统在出现不可预知故障的时候,允许损失部分可用性

- Soft state（软状态）

允许系统中的数据存在中间状态，并认为该状态不影响系统的整体可用性，即允许系统在多个不同节点的数据副本之间进行数据同步的过程中存在延迟。 

- Eventually consistent（最终一致性）三个

最终一致性强调的是系统中所有的数据副本，在经过一段时间的同步后，最终能够达到一个一致的状态。因此最终一致性的本质是需要系统保证最终数据能够达到一致，而不需要实时保证系统数据的强一致性。 

BASE是对CAP中一致性和可用性权衡的结果，BASE理论的核心思想是:

即使无法做到强一致性，但每个应用都可以根据自身业务特点，采用适当的方式来使系统达到最终一致性。

## 分布式事务

分布式事务是指事务的参与者、支持事务的服务器、资源服务器以及事务管理器分别位于不同的分布式系统的不同节点之上。例如在大型电商系统中，下单接口通常会扣减库存、减去优惠、生成订单 id, 而订单服务与库存、优惠、订单 id 都是不同的服务，下单接口的成功与否，不仅取决于本地的 db 操作，而且依赖第三方系统的结果，这时候分布式事务就保证这些操作要么全部成功，要么全部失败。本质上来说，分布式事务就是为了保证不同数据库的数据一致性。 

解决方案：

### **2PC**

### 3PC

### TCC





























