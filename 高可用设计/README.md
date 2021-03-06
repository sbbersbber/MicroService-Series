![高可用题图](https://s2.ax1x.com/2019/11/18/M60zp4.png)

# 测试与高可用保障

QoS(Quality of Service)，顾名思义，QoS 就是服务质量的缩写。QoS 概念最初源于网络，指一个网络利用各种基础技术，提供更好网络通信服务能力, 是网络的一种安全保障机制，是用来解决网络延迟和阻塞等问题的一种技术。但是，如今 QoS 概念已经被范化，不仅用于网络，也用来标识应用服务、基础技术、资源保障的能力和质量。

高可用架构并非基础架构本身，而是涵盖了多个维度，为了保障最终交付/部署可用性的策略、机制、技术架构的集合。质量保障应该是从团队组织，到开发，测试，发布，运维等全生命周期的工作，而不是某个孤立的技术突破点。

![mindmap](https://i.postimg.cc/zDK3YzGQ/image.png)

# 高并发应对

高并发系统的典型场景就是电商大促、12306 抢票等，瞬间洪峰超出最大负载，热点商品、票仓挤占正常流量，导致 CPU LOAD 居高不下，请求响应缓慢而损害用户体验。高并发场景下的挑战，首先是继承了我们在并发编程中讨论的挑战点，譬如共享资源的并发访问，计算型密集任务的分布式调度等。

在本篇的高并发应对中，我们核心是关注于单一热点资源的峰值流量的架构与策略，对于分布式计算、调度等相关内容，我们将会在[分布式基础架构](https://ng-tech.icu/DistributedSystem-Series/#/)系列中进行详细地讨论。

# 自动化运维

在 [DevOps 篇](https://ng-tech.icu/Backend-Series/#/devops)中我们详细讨论了运维的方法论、运维体系的搭建；本小节则再具体从自动化运维的角度，阐述其对于高可用保障的重要意义。

# About

## Copyright & More | 延伸阅读

![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)
![](https://parg.co/bDm)

笔者所有文章遵循 [知识共享 署名-非商业性使用-禁止演绎 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)，欢迎转载，尊重版权。如果觉得本系列对你有所帮助，欢迎给我家布丁买点狗粮(支付宝扫码)~

[![技术视野](https://s2.ax1x.com/2019/12/03/QQJLvt.png)](https://github.com/wx-chevalier/Awesome-MindMaps)

您还可以前往 [NGTE Books](https://ng-tech.icu/books/) 主页浏览包含知识体系、编程语言、软件工程、模式与架构、Web 与大前端、服务端开发实践与工程架构、分布式基础架构、人工智能与深度学习、产品运营与创业等多类目的书籍列表：

[![NGTE Books](https://s2.ax1x.com/2020/01/18/19uXtI.png)](https://ng-tech.icu/books/)
