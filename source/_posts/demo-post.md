---
title: Demo Post # 文章標題
# data: 2025-02-01 # 文章創建日期（不添加会采用文件创建日期）
# updated: 2025-02-01 # 文章更新日期（可选，不添加会采用文件修改日期）
description: Readme for how to use Hexo and Butterfly theme # 文章描述（可选）
categories:
  - Memo
tags:
  - Memo
katex: true
---


# Install

Refer to [Hexo Docs](https://hexo.io/zh-cn/docs/)

Node modules and butterfly theme are not in the repository, so need to install them separately.

## Ubuntu

Refer to [Butterfly Install](https://butterfly.js.org/posts/21cfbf15/)

![version note](version_note.png)

```bash
sudo apt-get install git

# Clone the repository, first set ssh correct
git clone git@github.com:Judera9/Blogs.git
# git clone https://github.com/Judera9/Blogs.git # Need to set token

# Install nvm for Managing Node.js Versions
wget -qO- https:/****/raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash 
source ~/.bashrc
nvm install 18
nvm use 18

# Install Hexo
npm install -g hexo-cli

# Install Butterfly Theme
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
npm install hexo-renderer-pug hexo-renderer-stylus --save

```

Test for Install, you can see the result in the browser with link [http://localhost:4000](http://localhost:4000).

```bash
hexo clean
hexo g # hexo generate
hexo s # hexo server
```

## Windows

TODO


# Install Dependencies

TODO





# Generate and deploy



# Functions

## Math

参考[Butterfly配置教程](https://butterfly.js.org/posts/4aa8abbe/#Math-%E6%95%B8%E5%AD%B8)，使用KaTeX渲染公式，需要确认是否已经按照要求安装对应的hexo插件，参考[KaTex官网](https://katex.org/docs/options.html)可以进一步调整配置，如果当前Blog需要渲染KaTex公式，需要在 Post Front-Matter 中设置`katex: true`。

```bash
npm un hexo-renderer-marked --save # 如果有安裝這個的話，卸載
npm un hexo-renderer-kramed --save # 如果有安裝這個的話，卸載

npm i hexo-renderer-markdown-it --save # 需要安裝這個渲染插件
npm install katex @renbaoshuo/markdown-it-katex #需要安裝這個katex插件
```

---

$$S_{i,j} = \sum_{i=1}^n a_i b_i + \epsilon$$

对于一般的圆$x^2+y^2+Dx+Ey+F=0$，其圆心坐标为$\left(-\cfrac{D}{2},-\cfrac{E}{2}\right)$，半径为$\cfrac{\sqrt{D^2+E^2-4F}}{2}$

$\begin{aligned} \sin 2\theta & = 2\sin \theta \cos \theta \\ & = \cfrac{2 \tan \theta}{1+\tan^2 \theta} \end{aligned}$

$$f(x) = \left\{ \begin{array}{ll} \lambda e^{-\lambda x}, & x>0 \\ 0, & others \end{array} \right. (\lambda > 0)$$

$A_{m,n} = \begin{pmatrix} a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\ a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\ \vdots & \vdots & \ddots & \vdots \\ a_{m,1} & a_{m,2} & \cdots & a_{m,n} \end{pmatrix}$

---

上面测试是否能够生成数学公式，如果使用`hexo s`无法显示说明安装有问题。

## Search

参考[Butterfly配置教程](https://butterfly.js.org/posts/4aa8abbe/?highlight=relate#%E6%90%9C%E7%B4%A2)目前测试了local_search，能够正常使用，不确定后续是否遇到加载速度问题，已用本科学生邮箱申请了[DocSearch](https://docsearch.algolia.com/apply/)，看后续是否有回复。

