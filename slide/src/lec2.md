---
title: First-year Master Learning
separator: <!--s-->
verticalSeparator: <!--v-->
theme: simple
highlightTheme: github
css: 
    - custom.css
revealOptions:
    transition: 'slide'
    transitionSpeed: fast
    center: false
    slideNumber: "c/t"
    width: 1000
---

<div class="middle center">
<div style="width: 100%">

<img src="lec1/jiangnan_logo.png" style="margin-bottom: 1em" width="40%">

# First-year Master Learning

<hr/>

1. Books
2. Papers
3. Projects

<!-- ←/→ Space Home End 翻页 -->


<div style="text-align: right; margin-top: 2em;">
<p>By冯则涛&emsp;2024.8.30&emsp;&emsp;&emsp;</p>
</div>

</div>
</div>

<!--s-->
<!-- .slide: data-background="lec2/background-books.png" -->

<div class="middle center">
<div style="width: 100%">

# 1. Books

<hr/>


- 1.1 多视图几何
- 1.2 视觉 SLAM 14 讲
- 1.3 三维数据场可视化


</div>
</div>

<!--v-->
<!-- .slide: data-background="lec2/background-books.png" -->

## 1.1 多视图几何

<!-- <center><img src="lec2/mvg-note.jpg" alt="mvg-note" width="80%"></center> -->

<div class="fragment">
<div class="mul-cols">
<div class="col">


#### 1.1.1 学习程度

- 2D、3D 射影几何和变换
- 摄像机模型、如何求解摄像机参数
- 单视图几何、多视图几何
    - 对极几何
    - 基础矩阵、本质矩阵、单应矩阵
- 算法评价和误差分析

</div>

<div class="col">

<div align="center">

<a href="https://note.jujimeizuo.cn/cv/mvg/">
<img src="lec2/mvg-note.jpg" alt="mvg-note" width="80%">
</a>

</div>
</div>
</div>
</div>
</div>

<div class="fragment">

#### 1.1.2 [SfM & openMVG](https://slides.jujimeizuo.cn/GroupMeetingReport/lec1/#/)

- 已知：三维场景的 m 张图像以及每张图像对应的摄像机内参数矩阵 $K_i(i=1,...,m)$
- 求解：
    - 三维场景结构，即三维场景点坐标$X_j(j=1,...,n)$；
    - m 个摄像机的外参数 $R_i$及$T_i(i=1,...,m)$
    - 结合相机内参重建稀疏点云

</div>

<!--v-->
<!-- .slide: data-background="lec2/background-books.png" -->

## 1.2 视觉 SLAM 14 讲

<div class="fragment">

#### 1.2.1 学习程度

</div>

<div class="fragment">

- 三维刚体运动（旋转矩阵、旋转向量、欧拉角、四元数、**Eigen**）
- 李群和李代数（$SO(3)$,$SE(3)$）
- 相机模型（单目、双目、RGB-D）
- 非线性优化
- 视觉里程计
    - 特征点法、特征提取和匹配、对极几何、三角测量、PnP（3D-2D）、ICP（3D-3D）
    - 直接法、光流法
- 后端优化
    - 卡尔曼滤波器
    - BA
    - 回环检测

</div>

<hr>

<div class="fragment">

#### 1.2.2 ORB-SLAM2 源码

</div>

<!--v-->
<!-- .slide: data-background="lec2/background-books.png" -->

## 1.3 三维数据场可视化

- 面绘制
- 体绘制
- [笔记链接](https://note.jujimeizuo.cn/cv/3d-visualization/)



<!--s-->
<!-- .slide: data-background="lec2/background-papers.png" -->

<div class="middle center">
<div style="width: 100%">

# 2. Papers

<hr/>

</div>
</div>


<!--v-->
<!-- .slide: data-background="lec2/background-papers.png" -->



<!--s-->
<!-- .slide: data-background="lec2/background-projects.png" -->

<div class="middle center">
<div style="width: 100%">

# 3. Projects

<hr/>

- 3.1 大模型学习
- 3.2 图像编辑器

</div>
</div>


<!--v-->
<!-- .slide: data-background="lec2/background-projects.png" -->

## 3.1 大模型学习

<div class="fragment">

#### 3.1.1 学习内容

</div>

<div class="mul-cols">
<div class="col">

<div class="fragment">

- NLP
    - Word2Vec
    - RNNs
        - RNN
        - **LSTM**（可用于视频序列）
        - GRU

</div>

<hr>

<div class="fragment">

- Transformer 系列
    - Attention
    - Transformer
    - BERT（预训练 + 微调）
    - ViT

</div>
</div>

<div class="col">

<div class="fragment">

- Tools
    - Langchain
    - XTuner（微调）
    - LMDeploy（量化部署）
    - OpenCompass（测评）
    - MetaGPT（Agent）

</div>

<hr>

<div class="fragment">

<div align="center" style="font-size:15px">

<a href="https://note.jujimeizuo.cn/llm/internlm/">
<img src="lec2/camp.png" alt="camp" width="100%">
</a>

</div>
</div>
</div>

<!--v-->
<!-- .slide: data-background="lec2/background-projects.png" -->

## 3.1 大模型学习

<div class="fragment">

#### 3.1.2 项目

</div>

<div class="fragment">
<div class="mul-cols">

<div class="col" style="margin-left: 50px;">
<a href="https://github.com/SmartFlowAI/EmoLLM">
<img src="lec2/emollm.jpg" alt="emollm" width="100%">
</a>

用 GLM-4 微调心理相关数据集

</div>

<div align="center">
<div class="col">

<img src="lec2/emollm-framework.png" alt="emollm-framework" width="80%">

</div>
</div>
</div>
</div>

<hr>

<div class="fragment">
<div class="mul-cols">
<div class="col">

基于 MetaGPT 构建智能体应用，利用 Agent 架构得到更丰富、更定制化详细的回答

</div>

<div class="col" style="margin-right: -100px;">

<a href="https://github.com/SocialAI-tianji/Tianji">
<img src="lec2/tianji.jpg" alt="tianji" width="70%">
</a>

</div>
</div>
</div>

<!--v-->
<!-- .slide: data-background="lec2/background-projects.png" -->

## 3.2 图像编辑器

<div class="fragment">

#### 3.2.1 前期调研

</div>


<div class="fragment">

- 调研开源项目，代码不能采用粘合代码调用，每个功能模块都必须有。**GIMP、minPaint、Inkscape**
- 最终选择 Inkscape 作为基础框架，进行二次开发

</div>

<div class="fragment">

- [开发文档](https://tg4663zohn.feishu.cn/docx/WLh4dOr1FoZT70x1uvWchf25nne?pre_pathname=%2Fdrive%2Fme%2F)

<div align="center">

<img src="lec2/foy-feishu.jpg" alt="foy-feishu" width="58%">

</div>
</div>

<!--v-->
<!-- .slide: data-background="lec2/background-projects.png" -->

### 3.2 图像编辑器

<div class="fragment">

#### 3.2.2 开发过程

把每个模块、每个类的作用、相互嵌套关系，都列举出来，研究。

<div align="center">

<img src="lec2/foy-framework.jpg" alt="foy-framework" width="50%">

</div>
</div>

<div class="fragment">

#### 3.2.3 现场调研

去线下收集功能，并给老师们做汇报。

</div>


<!--s-->

<div class="middle center">
<div style="width: 100%">

# 谢谢大家

<hr/>

**Question?**

</div>
</div>