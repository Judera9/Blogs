---
title: "[Paper Reading] High performance RL Sim-to-Real on Spot"
description: 这篇论文研究了面向强化学习的Sim-to-Real迁移学习模块，实验基于Boston Dynamics的Spot机器人平台。研究重点集中于执行器参数建模，采用数据驱动的领域自适应方法优化电机参数。
date: 2025-05-16T18:21:14.655Z
top_img: false
cover: ./img/Robotics/20250516/2025-05-16-02.png
tags:
  - Robotics
  - Sim-to-Real
  - RL
categories:
  - - Robotics
    - Paper-Reading
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

1. 这篇论文研究了面向强化学习的Sim-to-Real迁移学习模块，实验基于Boston Dynamics的Spot机器人平台。研究重点集中于**执行器参数建模**，采用数据驱动的领域自适应方法优化电机参数。需要指出的是，本文方法主要解决由执行器特性引起的仿真-现实差异，而对于环境因素或刚体模型不准确等其他因素导致的Sim-to-Real Gap则未涉及。这篇工作主要探讨以下两个核心问题：
    * 如何量化仿真环境与真实环境中的策略分布差异（scoring metrics）
    * 如何使策略适应从真实机器人获取的数据分布（algorithm）
2. 机器人Sim-to-Real问题及其研究进展：
    * Sim-to-Real问题是随着基于学习的控制方法在机器人领域兴起而产生的重要研究课题。在IsaacSim或MuJoCo等仿真环境中，策略训练完全依赖于仿真器的刚体动力学模型。然而，这些仿真器通常会对物理计算进行必要的简化处理。此外，真实机器人的质量、惯量、摩擦系数等参数与仿真环境难以完全匹配，这些因素共同导致了Sim-to-Real Gap。
    * 数学上，**Sim-to-Real Gap可以看作是策略在仿真与真实环境下的动力学表现不同，即我们在强化学习中所说的State Transition $p_{\theta}(s_{t+1} | s_t, a_t)$的差异**。目前解决Sim-to-Real Gap的方法主要有以下几种：
      * 知识蒸馏 （RMA, Teacher-Student）
      * 域随机化（Domain Randomiazation）
      * 域自适应（Domain Adaptation）
      * 基于模型的强化学习（Model-based RL）
      * 元学习（Meta Learning）

<!-- FM:Snippet:Start data:{"id":"img","fields":[]} -->
<div style="text-align: center;">
  <img src="../../../../../img/Robotics/20250516/2025-05-16-01.png" alt="Sim2Real Methods" style="max-width: 80%; height: auto; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <p style="color: #666; font-style: italic; margin-top: 8px;">Sim2Real Methods<sup>1</sup></p>
</div>
<!-- FM:Snippet:End -->

## 技术方法

<!-- FM:Snippet:Start data:{"id":"img","fields":[]} -->
<div style="text-align: center;">
  <img src="../../../../../img/Robotics/20250516/2025-05-16-02.png" alt="Pipeline" style="max-width: 100%; height: auto; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <p style="color: #666; font-style: italic; margin-top: 8px;">Spot Train and Deployment Pipeline</p>
</div>
<!-- FM:Snippet:End -->

<!-- FM:Snippet:Start data:{"id":"img","fields":[]} -->
<div style="text-align: center;">
  <img src="../../../../../img/Robotics/20250516/2025-05-16-06.png" alt="Framework" style="max-width: 90%; height: auto; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <p style="color: #666; font-style: italic; margin-top: 8px;">Framework of this paper</p>
</div>
<!-- FM:Snippet:End -->

<!-- FM:Snippet:Start data:{"id":"img","fields":[]} -->
<div style="text-align: center;">
  <img src="../../../../../img/Robotics/20250516/2025-05-16-07.png" alt="Method" style="max-width: 100%; height: auto; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <p style="color: #666; font-style: italic; margin-top: 8px;">Method of this paper</p>
</div>
<!-- FM:Snippet:End -->


* **整个工作包含如下几个部分，整合为一个完整的Sim-to-Real框架**
(1) 首先使用默认参数训练一个基础policy，从真机和仿真中的spot机器人上收集数据，包括策略的观测数据，以及Spot提供的其他接口数据（虽然有力反馈，但没使用）
(2) 首先对Sim-To-Real问题进行建模，定义了三个Gap的具体形式，分别是通信延迟，策略观测噪声和执行器参数，然后通过真机获取的相关数据前两种Gap直接建模得到相关超参数，然后电机模型相关参数则用优化方法进行确认；
(3) 然后使用关节的位置和速度作为特征，使用Wasserstein Distance和MMD来设计scoring function，在仿真中固定策略模型，通过CMA-ES算法优化电机模型参数
(4) 使用优化后的电机模型参数重新训练policy，并在真机上进行验证

## 实验结果

* 实验部分通过实现了一个flight phase gait的速度跟踪来作为衡量Sim-To-Real效果的指标，下面是plot出来的结果，可以看到使用优化后的参数，策略的动作会没有之前保守，并且Simulation和Hardware之间的差距减小了，还是有明显效果的。

<div class="image-gallery">
  <!-- 第一行图片 -->
  <div class="image-row">
    <div class="image-container">
      <img src="../../../../../img/Robotics/20250516/2025-05-16-03.png" alt="pic1">
      <p class="image-caption">Policy Action Comparison</p>
    </div>
    <div class="image-container">
      <img src="../../../../../img/Robotics/20250516/2025-05-16-04.png" alt="pic2">
      <p class="image-caption">Policy Troque Comparison</p>
    </div>
  </div>

  <!-- 第二行图片 -->
  <div class="image-row">
    <div class="image-container">
      <img src="../../../../../img/Robotics/20250516/2025-05-16-05.png" alt="pic3">
      <p class="image-caption">Base Forward Linear Velocity Comparison</p>
    </div>
  </div>

  <!-- 可以继续添加更多行和图片 -->
</div>

## 论文总结

1. 首先，这个工作研究的问题是非常有价值的，RL作为一个强大的工具，但是极度依赖于仿真的结果，因此有偏的模型会严重影响RL在真机上的效果
2. 个人感觉论文提出的方法还是非常靠谱的，只不过稍微有点复杂了，并且似乎也无法保证每台机器用一组参数就能够保证Sim-to-Real Gap的减小，可能对同一机器人的每台机器还需要重新训练优化参数
3. 可能更好的方法还是Meta Learning或Model-based RL这一类方法，使用通用模型或通用算法来适应不同的环境，而不是通过单独一个policy的结果来优化仿真参数

## 引用参考

[1] https://arxiv.org/pdf/2009.13303/1000

<!-- http://localhost:4000/Blogs/2025/05/16/Robotics/2025-05-16-paper-of-high-performance-spot-sim2real -->