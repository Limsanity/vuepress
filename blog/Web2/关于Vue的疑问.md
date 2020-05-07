---
sidebar: auto
title: '关于Vue的疑问'
---

# 关于Vue的疑问

## 为什么patch时将当前需要处理的节点插入到旧树中未处理的节点之前

该问题来自《深入浅出VueJS》，并且在此书patch章节中的更新策略给出了解答。

如果将当前需要处理的节点插入到旧树中已处理的节点之后，可能造成位置错乱。假设当前需要处理的节点A在旧树中不存在时，将其插入到旧树已处理节点之后，那么处理下一个节点B时，如果B仍然不存在于旧树中，那么此时的节点B插入到旧树已处理节点之后就造成B在A之前的错乱。



## Vue Composition API和Slot复用对比

[推荐阅读](https://logaretm.com/blog/the-case-for-hoc-vs-composition-api/)

结论：

- 如果不需要扩展slot中的props，就使用slot进行复用，composition api对与prop扩展可以利用js的灵活性
- 如果在同一组件中会对相同功能使用多次，就使用slot，因为每个slot的作用域都是独立的，无需为变量重命名，而composition api需要对变量进行重命名，可能带来繁琐