---
title: 'PostgreSQL'
sidebar: auto
---

# PostgreSQL

[PostgreSQL开源社区官方网址](https://www.postgresql.org)

[PostgreSQL中文社区官方网址](https://www.postgres.cn)



## 表

### 约束

- CHECK约束
  - 字段级约束
  - 指定特定名字的字段级约束
    - 可清楚地识别出错误信息
  - 表级约束
- 非空约束
- 唯一性约束
- 主键约束
- 默认约束
- 外键约束

### 删除数据表

- 删除主表只会删除子表的外键引用约束，并不会删除子表及其记录。



## 数据类型

- 类型运算符

### 日期

- 特殊值**now**、**today**、**tomorrow**、**yesterday**在被系统读到时会被转换为一个指定的时间值。

### 几何类型

### JSON数据类型

- json：处理函数每次执行必须重新解析该数据。

- jsonb：输入该类型稍慢些，但是数据处理要快很多。支持索引。


### 范围类型

### 数组类型

- 数组下标从1开始。