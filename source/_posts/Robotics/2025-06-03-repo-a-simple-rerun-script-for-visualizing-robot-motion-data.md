---
title: "[Repo] Use ReRun for visualizing robot motion data"
description: Provide a easy-to-use script for visualizing robot motion data
date: 2025-06-03T16:24:53.920Z
tags:
    - Robotics
    - Imitation Learning
categories:
    - - Robotics
      - Tools
katex: false
data: null
updated: null
top_img: false
cover: ./img/Robotics/20250603/2025-06-03-01.jpg
---


## Introduction

---

**Repository Link:** https://github.com/Judera9/RerunVis
**Repository Introduction:** Recently I have been working on ***robot imitation learning***, this script is written on top of Unitree Lafan repository. I add several features to make it easy to use for visualizing robot motion data for newly added robot assets. I use ***pinnoccchio*** to load the robot model and calculate the forward kinematics, and use ***rerun*** to visualize the robot motion data. The extended dataset can be saved as a `.npy` file, and be used in RL training. I also add a demo for unitree g1 robot, you can follow the README.md to run it.

<!-- FM:Snippet:Start data:{"id":"img","fields":[]} -->
<div style="text-align: center;">
  <img src="/Blogs/img/Robotics/20250603/2025-06-03-01.jpg" alt="demo for unitree g1 robot" style="max-width: 80%; height: auto; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <p style="color: #666; font-style: italic; margin-top: 8px;">Demo for Unitree G1 Robot</p>
</div>
<!-- FM:Snippet:End -->

---

## Install

After cloning the repo:

```bash
conda create -n rerun_test python=3.10
conda activate rerun_test
pip install -e .
conda install pinocchio -c conda-forge
```

Test install:
- should show a window with a g1 jumping
- can save a converted motion npy file

```bash
python scripts/rerun_visualize.py
```

## Usage

[**IMPORTANT**] Because current used DEMO g1 data have 29 dof, the DEMO g1 urdf has 23 dof. So you should remove the following codes in `utils/data_utils.py` for your robot:

```python
joint_pos = np.delete(
    joint_pos, (13, 14, 20, 21, 27, 28), axis=1
)  # WARNING
```

---

1. Add your robot urdf and meshes in `assets/`
2. Add your own robot config in `config/`
3. Change the loaded robot config in `scripts/rerun_visualize.py`

## Data Format

* Original data format support `.csv` and `.npy` files.
* Both files is the same format described as following, this can be aware from the function `load_motions_for_rerun` in `utils/data_utils.py`

```csv
...
m_{t-1} -> ROOT_POS(3), ROOT_ORI(4), JOINT_POS(NUM_JOINTS)
m_t -> ROOT_POS(3), ROOT_ORI(4), JOINT_POS(NUM_JOINTS)
...
```

* The converted data format is `.npy` file.
* The converted data include both joint space states and keypoints states, which are calculated using Pinocchio. You can try using different frame (*LOCAL, WORLD, LOCAL_WORLD_ALIGNED*) to calculate the keypoints states.
* details can also be found in the function `load_motions_for_rerun` in `utils/data_utils.py`

