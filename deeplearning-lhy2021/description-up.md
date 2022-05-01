---
title: 'description-up'
date: 2021-09-06 10:00
categories:
- Deep Learning
tags:
- lhy2021
---
# Start
今天开始学习接触deep learning,一个全新的领域，通过一定的资料了解，接触到李宏毅老师的机器学习课程，仅以此系列Blog做一下笔记总结。
# 笔记
## 1.机器学习两大分类

> Regression: 回归
> Classification: 分类

## 2.机器学习找到一个Function的过程分为三个步骤

1.Function with Unknown Parameters
   写出一个带有位置参数的函式 
   **Model**  y = b + wx<sub>1</sub> based on domain knowledge
   x<sub>1</sub>已知的叫做**feature**,w相乘的叫做**weight**,加上的b叫做**bias**
2.Define Loss form Training Data
   定义一个Function Loss
   Loss is a function of parameters **L(b,w)**
   Loss: how good a set of values is.

   真实值**Label**与预测值之间的误差e，Loss: $L\frac{1}{N}\sum_n{e_n}$
3.Optimization
   解一个最佳化问题，找到最佳的w*和b*使Loss最小
   首先找到一个初始w值w<sub>0</sub>，计算w<sub>0</sub>处斜率，之后向loss减小方向变化w，变化幅度受斜率影响和学习速率(learning rate)影响,学习速率由自己设定。机器学习中自己设置的参数叫做hyperparameters。
