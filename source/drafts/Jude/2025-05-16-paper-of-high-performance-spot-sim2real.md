---
title: "[Paper Reading] High performance RL Sim-to-Real on Spot"
description: ""
date: 2025-05-16T18:21:14.655Z
preview: ""
draft: true
tags:
    - Robotics
    - Sim-to-Real
    - RL
categories:
    - Robotics
katex: true
---

## 基础信息

---

**论文题目：** High-Performance Reinforcement Learning on Spot: Optimizing Simulation Parameters with Distributional Measures
**作者：** A.J. Miller, Fangzhou Yu, Michael Brauckmann, and Farbod Farshidian
**工作单位：** RAI Institute, Cambridge
**发表时间：** ICRA 2025
**论文链接：** https://rai-inst.com/resources/papers/high-performance-reinforcement-learning-on-spot/

---

## 研究问题

1. 本文研究的问题是面向Reinforcement Learning的Sim-to-Real模块，硬件实验部份基于Boston dynamics的Spot机器人完成；本文主要还是侧重Actuator参数建模，通过data-driven的Domain Adaptation方法来实现部份电机参数的优化，针对如环境因素，或刚体动力学导致的Sim-to-Real Gap，论文中的方法应该无法解决。本文关心的重点是：
    * 如何量化simulation和policy的分布的差异
    * 如何让policy适应从真机上获取的数据的分布情况
2. 什么是机器人的Sim-to-Real问题，之前有哪些工作在解决Sim-to-Real Gap？
    * 机器人的Sim-to-Real问题，是在机器人领域当下learning-based控制方法成为主流后产生的一个重要课题。由于在我们训练的仿真器中，如IsaacSim或MuJoCo，机器人的仿真依赖于


3. RL Sim2Real方法
  1. Knowledge Distillation (RMA, T-S)
  2. Domain Randomiazation
  3. Domain Adaptation (Transfer Learning, Model-based RL)
  4. Meta Learning

## 实验结果



## 论文总结