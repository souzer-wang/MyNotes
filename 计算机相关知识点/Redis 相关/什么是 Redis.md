# 什么是 Redis

本质上就是一个 Key - Value 类型数据库

整个数据库加载在内存上进行操作，定期通过异步操作把数据库数据 flush 到硬盘上进行保存

# Redis 的主要特性

[redis的八大特性_恒星v的技术博客_51CTO博客](https://blog.51cto.com/u_13581826/2308263)

- 性能出色： 所有数据加载到内存中，所以速度非常快
- 支持保存多种数据结构
- 支持持久化：可以将内存中的数据保存在磁盘中，重启的时候可以再次加载使用
- 支持多种语言
- 简单稳定
  - 源码少
  - 使用单线程模型
  - 不依赖系统类库
- 主从复制：可实现多个相同数据的 Redis 副本，复制功能是分布式 Redis 的基础

# Redis 各类知识点汇总

[目前最全的Redis知识点及面试题汇总_shuningzhang的专栏-CSDN博客_redis知识点](https://blog.csdn.net/shuningzhang/article/details/90667395)