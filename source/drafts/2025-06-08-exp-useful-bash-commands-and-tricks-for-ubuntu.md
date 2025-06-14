---
title: "[Exp] Useful bash commands and tricks for Ubuntu"
description: ""
date: 2025-06-08T04:15:26.097Z
preview: ""
draft: true
tags: []
categories: []
katex: false
data: null
updated: null
top_img: false
cover: ""
---

## Ubuntu proxy settings

```bash
# export global proxy in .bashrc
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890

# unset proxy
unset http_proxy
unset https_proxy

# set git proxy
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890

# echo global proxy
echo $http_proxy
echo $https_proxy
```

## Killing zombie program

```bash
# find zombie process
ps aux | grep 'name'
kill -9 PID
```

## Conda and pip settings

```bash
# change pip source to tsinghua
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

# change conda source to tsinghua
conda config --add channels https://pypi.tuna.tsinghua.edu.cn/simple
```

## Install isaacsim & IsaacLab env for Ubuntu 22.04

```bash
# Install isaacsim
conda create -n env_isaaclab python=3.10
conda activate env_isaaclab
pip install torch==2.5.1 torchvision==0.20.1 --index-url https://download.pytorch.org/whl/cu118
pip install --upgrade pip
pip install 'isaacsim[all,extscache]==4.5.0' --extra-index-url https://pypi.nvidia.com
isaacsim  # verify installation

# Install IsaacLab
cd /path/to/IsaacLab
conda activate env_isaaclab
./isaaclab.sh -i
python scripts/tutorials/00_sim/create_empty.py  # verify installation
./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Velocity-Rough-Anymal-C-v0 --headless  # verify installation
```

## Usage of ffmpeg

## Vscode settings and launch file

add file here for downloading...

## Setup applications for binary files

```bash
```



