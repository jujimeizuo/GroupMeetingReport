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
- 1.2 三维数据场可视化


</div>
</div>

<!--v-->
<!-- .slide: data-background="lec2/background-books.png" -->

## 1.1 多视图几何

<!-- <center><img src="lec2/mvg-note.jpg" alt="mvg-note" width="80%"></center> -->

<div class="mul-cols">
<div class="col">

- 2D、3D 射影几何和变换
- 摄像机模型、如何求解摄像机参数
- 单视图几何、多视图几何
    - 对极几何
    - 基础矩阵、本质矩阵、单应矩阵
- 算法评价和误差分析

</div>
<div class="col">
<div align="center" style="font-size:15px">

<a href="https://note.jujimeizuo.cn/cv/mvg/">
<img src="lec2/mvg-note.jpg" alt="mvg-note" width="80%">
</a>

</div>
</div>
</div>

- [SfM & openMVG](https://slides.jujimeizuo.cn/GroupMeetingReport/lec1/#/)
    - 已知：三维场景的 m 张图像以及每张图像对应的摄像机内参数矩阵 $K_i(i=1,...,m)$
    - 求解：
        - 三维场景结构，即三维场景点坐标$X_j(j=1,...,n)$；
        - m 个摄像机的外参数 $R_i$及$T_i(i=1,...,m)$
        - 结合相机内参重建稀疏点云

<!--v-->
<!-- .slide: data-background="lec2/background-books.png" -->

## 1.2 三维数据场可视化

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



<!--s-->

<div class="middle center">
<div style="width: 100%">

# 谢谢大家

<hr/>

**Question?**

</div>
</div>