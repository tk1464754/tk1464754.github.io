---
layout: post
title:  "微服务"
categories: 微服务
tags:  微服务
author: zz
---

微服务之间如何通信？

同步调用

rest    对外  基于http

rpc     对内   远程过程调用

异步非阻塞通信

Netty -> NIO,AIO      nio异步并阻塞  aio异步非阻塞

http  -   应用层  跨防火墙 ， 在不同的局域网内通信

RPC  -    远程过程调用，基于TCP ，传输层。 有点 ：底层 速度快  缺点不能夸防火墙

zookeeper 服务注册与发现     单点故障 解决 分布式锁

多个master节点提供服务 主从模式 当主master因为网络震荡原因 无法收到心跳包,另外一个从节点升为主节点 可能出现双master问题

解决: 所有节点都向zookeeper进行注册  zookeeper选举 编号最小的成为主节点  会带一个锁 当一个master挂掉 自动选举新的主节点  挂掉的在zk中删除  恢复后重新注册成为从节点

分布式锁解决另外一个问题:用户下单 ,有三台服务器 每个服务器都有变量A 不存在共享 是有问题的  

例如3个用户下单购买  库存只有2  最终2-3=-1 用zk可以实现分布式锁

分布式事务 最终一致性  支付宝DTS框架



Redis的五种数据类型：字符串（string）、列表（list）、哈希(hash)、集合（set）、有序集合（sorted sets）。

zuul api路由转发 和 过滤器	



jvm是java虚拟机

java代码需要运行在jvm之上

java语言既是编译型语言也是解释性语言

通过JVM可以跨平台

java运行时有一个编译阶段，编译完成之后  JVM会把字节码装载进来，通过classloader。

JVM运行完成 正确读取到了java字节码	就正常运行起来了 

当运行到一定时间后	有一些对象的引用已经无效了 于是就有一个GC的垃圾回收机制



spring cloud   微服务解决方案

1 spring cloud Netflix

API网关，zuul组件 

服务注册于发现  Eureka

Feign  -  http client  -http通信

熔断机制Hystrix

2 Apache Dubbo zookeeper（不完善）2.6系列  3代孵化中

Dubbo是一款高性能的RPC 通信框架

API网关，没有

服务注册与发现 zookeeper

服务挂了（熔断机制）没有

spring cloud Alibaba   微服务开发一站式解决方案

API网关，

服务注册与发现  Nacos  +限流



MQ中间件

解耦 异步 削峰

如何保证MQ幂等性(重复消费):1如果是插入DB,添加唯一id,就要判断下 存在就update 2redis用set 天然幂等性 3唯一主键约束,插入报错

kafka生产消息会有一个offset代表消息顺序序号,消费者消费提交offset 通过zookeeper记录offset=几的那条

定时定期offset

数据丢失:1trycatch同步事务机制 2 confirm机制 消费后回调生产者接口ACK 3rabbitMQ本身挂掉  开启磁盘持久化



分布式事务

两阶段提交(单系统操作多库方案)  效率低 依赖数据库 不适合分布式

例如数据库ABC 每个库都OK才会提交  只要有一个不行,就回滚  

TCC事务补偿(try confirm cancel) 需要手写大量代码, 对一致性要求很高

银行A转账银行B  1先try  冻结响应资金  2 A减少B增加 如果错误 手动写代码退回   3 回滚



最终一致性(rocketMQ自带)

1 首先A系统发送一条prepare(准备)消息  然后A系统执行DB操作  本地事务成功后发送confirm 否则回滚prepare

2 如果发送confirm失败 消息停留在prepare, rocketMQ会自动轮询所有prepare消息,然后回调A系统接口 是重新发送还是回滚

3 发送confirm后 MQ通知B系统 消费  消费后本地操作DB 开始本地事务

4 B系统消费失败  解决方案: 1通过MQ机制(API交互) 让MQ重发  2 通知zk zk通知A重发 (B系统保持幂等性 1 DB唯一键约束, ZK创建node)

实际可能不做分布式事务(性能差,复杂) 就是A调用BCD 报错会打印日志

不做事务方案: 1监控 报错发邮件之类 2 打印日志 3定位排查 解决 4 人工修复数据
