---
title: "[Paper Reading] High performance RL Sim-to-Real on Spot"
description: 这篇论文研究了面向强化学习的Sim-to-Real迁移学习模块，实验基于Boston Dynamics的Spot机器人平台。研究重点集中于执行器参数建模，采用数据驱动的领域自适应方法优化电机参数。
date: 2025-05-16T18:21:14.655Z
top_img: false
cover: ./img/Robotics/20250516/2025-05-16-02.png
draft: true
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

1. 这篇论文研究了面向强化学习的Sim-to-Real迁移学习模块，实验基于Boston Dynamics的Spot机器人平台。研究重点集中于执行器参数建模，采用数据驱动的领域自适应方法优化电机参数。需要指出的是，本文方法主要解决由执行器特性引起的仿真-现实差异，而对于环境因素或刚体模型不准确等其他因素导致的Sim-to-Real Gap则未涉及。这篇工作主要探讨以下两个核心问题：
    * 如何量化仿真环境与真实环境中的策略分布差异（scoring metrics）
    * 如何使策略适应从真实机器人获取的数据分布（algorithm）
2. 机器人Sim-to-Real问题及其研究进展：
    * Sim-to-Real问题是随着基于学习的控制方法在机器人领域兴起而产生的重要研究课题。在IsaacSim或MuJoCo等仿真环境中，策略训练完全依赖于仿真器的刚体动力学模型。然而，这些仿真器通常会对物理计算进行必要的简化处理。此外，真实机器人的质量、惯量、摩擦系数等参数与仿真环境难以完全匹配，这些因素共同导致了Sim-to-Real Gap。
    * 数学上，Sim-to-Real Gap可以看作是策略在仿真与真实环境下的动力学表现不同，即我们在强化学习中所说的State Transition $p_{\theta}(s_{t+1} | s_t, a_t)$的差异。目前解决Sim-to-Real Gap的方法主要有以下几种：
      * 知识蒸馏 （RMA, Teacher-Student）
      * 域随机化（Domain Randomiazation）
      * 域自适应（Domain Adaptation）
      * 基于模型的强化学习（Model-based RL）
      * 元学习（Meta Learning）

<!-- FM:Snippet:Start data:{"id":"img","fields":[]} -->
<div style="text-align: center;">
  <img src="../../../../../img/Robotics/20250516/2025-05-16-01.png" alt="Sim2Real Methods" style="max-width: 80%; height: auto; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <p style="color: #666; font-style: italic; margin-top: 8px;">Sim2Real Methods<sup>2</sup></p>
</div>
<!-- FM:Snippet:End -->

## 技术方法

<!-- FM:Snippet:Start data:{"id":"img","fields":[]} -->
<div style="text-align: center;">
  <img src="../../../../../img/Robotics/20250516/2025-05-16-02.png" alt="Framework" style="max-width: 100%; height: auto; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <p style="color: #666; font-style: italic; margin-top: 8px;">Framework</p>
</div>
<!-- FM:Snippet:End -->

1. 整体实现流程：
  1. 首先训练一个基础的policy，从真机和仿真中的spot机器人上收集数据，使用Wasserstein Distance和MMD来量化分布差异
  2. 利用两个metrics来设计scoring function，通过CMA-ES算法训练提升RL中使用的机器人模型的准确性
  3. 使用新的optimized spot model来重新训练policy
2. 机器人模型（actuator model）：
  1. 有力反馈，所以不需要考虑gear ratios等问题
  2. 在下层API调用时的Communication delay
[Image]
  3. Actuator建模
    - 膝关节有一个remotized lead-screw joint（远程丝杠关节），丝杠结构通过机械传动放大扭矩，但是会有更高的摩擦阻力，在仿真中需要精确的对丝杠摩擦力建模
    - 关节扭矩限制依赖关节位置和速度，通过二维曲线建模
[Image]
  4. Observation noise观测噪声建模
    - 使用data driven methods构建一个gaussain noise model，在训练的时候加在obs上
    - gaussian的mean和std是通过收集真机数据，然后FFT转换到频域，然后通过乘上频率，将不同频率的噪声进行归一化，这样能够统一对于所有频段的噪声进行建模，随后通过选择$$[\frac{f_s}{4},\frac{f_s}{2}]$$的中高频段噪声进行建模，计算出所建模噪声的std
3. 仿真模型参数的scoring
  1. 现有方法：
    1. 手动建模参数（system ID）：复杂度太高，不同系统的差异较大，无法迁移
    2. Actuator Network：认为black box不可靠，会混淆真实的情况；除此之外很多机器没有力反馈，无法闭环
  2. 论文采用的方案：
    1. 用一个gray box来参数化建模（主要针对joint friction和torque-speed limis一共8个超参数）
[Image]
    1. socring：如何评估参数的“好坏”是一个难题，论文使用了generative learning的一些statistical measures，将仿真中的agent看作generator，然后真机数据是real data，通过计算WD和MMD来捕捉分布的差异，其中WD擅长捕捉分布的全局形状结构差异（对重叠程度和偏移敏感），MMD捕捉高维空间中的局部细节差异
    2. optimization：使用CMA-ES无梯度算法（因为他们以前在其他协同设计的工作中用过）
  1. 方案使用的算法详情：
    1. Wasserstein Distance：可以用作一种distribution之间的距离的度量，但本质是计算distribution的Transpotation运输成本 $$T:p(y)=T(p(x))$$，WD距离难以计算，只有dim=1或高斯分布能够显式计算
[Image]
    2. Maximum Mean Discrepancy：将原始数据映射到高维特征空间（如通过核函数），在新空间中比较两组数据的均值差异
[Image]
    3. CMA-ES：假设目标分布是高斯分布
[Image]
[Image]
[Image]
[Image]
[Image]


## 实验结果

实验部分通过实现了一个flight phase gait的速度跟踪来作为衡量Sim-To-Real效果的指标，

<div class="image-gallery">
  <!-- 第一行图片 -->
  <div class="image-row">
    <div class="image-container">
      <img src="../../../../../img/Robotics/20250516/2025-05-16-03.png" alt="图片1">
      <p class="image-caption">图片1描述</p>
    </div>
    <div class="image-container">
      <img src="../../../../../img/Robotics/20250516/2025-05-16-04.png" alt="图片2">
      <p class="image-caption">图片2描述</p>
    </div>
  </div>

  <!-- 第二行图片 -->
  <div class="image-row">
    <div class="image-container">
      <img src="../../../../../img/Robotics/20250516/2025-05-16-05.png" alt="图片3">
      <p class="image-caption">图片3描述</p>
    </div>
  </div>

  <!-- 可以继续添加更多行和图片 -->
</div>



## 论文总结

1. 另外一个问题是，这种方法无法像论文中所述end-to-end的实现RL Sim-to-Real，因为这种方法所用到的数据量太少了，并且数据来源于一个预训练的policy，当policy发生偏移后，所训练得到的参数是否还是最优的，这一点值得商榷
2. 从这个角度看，潜在更好的方式可能还是Meta Learning或Model-based RL这一类方法，使用通用模型或通用算法来适应不同的环境，而不是通过单独一个policy的结果来优化仿真参数

## 引用参考

[1] A.J. Miller, Fangzhou Yu, Michael Brauckmann, and Farbod Farshidian. High-Performance Reinforcement Learning on Spot: Optimizing Simulation Parameters with Distributional Measures. In ICRA, 2025.

[2] https://arxiv.org/pdf/2009.13303/1000

<!-- http://localhost:4000/Blogs/2025/05/16/Robotics/2025-05-16-paper-of-high-performance-spot-sim2real -->