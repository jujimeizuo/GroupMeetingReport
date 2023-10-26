---
title: MVG 理论基础 & SFM 代码复现
separator: <!--s-->
verticalSeparator: <!--v-->
theme: simple
highlightTheme: monokai-sublime
css: 
    - custom.css
    - dark.css
revealOptions:
    transition: 'slide'
    transitionSpeed: fast
    center: false
    slideNumber: "c/t"
    width: 1000
---

<!-- 
复现一下基于SFM的代码（理论可以参考一下B站上 北京邮电大学 鲁鹏老师的三维重建的课程），然后月底的时候会进行一次汇报。汇报内容：掌握的多视图三维重建的理论基础、基于SFM的代码复现进度（如 openMVG等，第一阶段可以不需要后端优化的内容，只要能拿着几张视图可以恢复出相机的位姿以及恢复的稀疏点云即可）。
 -->


<!-- .slide: data-background="lec1/cover.png" -->

<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 大纲

- MVG 理论基础
- SFM 基础内容
- SFM 代码复现

<!--s-->
<!-- .slide: data-background="lec1/background.png" -->

<div class="middle center">
<div style="width: 100%">

# Part.1 MVG 理论基础


</div>
</div>


<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 基础知识

- 齐次坐标
- （齐次/非）线性方程组的最小二乘解
- 变换

<div class="three-line">

| 变换     | 2D  | 3D  |
| -------- | --- | --- |
| 欧式变换 | 3   | 6   |
| 相似变换 | 4   | 7   |
| 仿射变换 | 6   | 12  |
| 透视变换 | 8   | 15  |

</div>


<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 摄像机几何

- 像平面到像素平面：摄像机的投影矩阵 $M$ 使得 $P^\prime=MP$（11 DOF）
- 摄像机偏斜：$M$ 中需要一个 $\theta$ 的矫正
- 摄像机坐标系下的摄像机模型
    - $P^\prime=MP=K[I \ \ 0]P$
    - $K$ 为摄像机的内参数矩阵（5 DOF），内参数矩阵决定了摄像机坐标系下空间点到图像点的映射
- 世界坐标系转到摄像机坐标系
    - $P^\prime=K[R \ \ T]P_w=MP_w$
    - $[R \ \ T]$ 为外参数矩阵（6 DOF）


<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 摄像机标定

- 标定目标：从 1 张或多张图像中估算摄像机内、外参数矩阵
- 标定问题：
    - 世界坐标系中 $P_1, ..., P_n$ 位置已知
    - 图像中 $p_1, ..., p_n$ 位置已知
    - 计算摄像机内、外参数
- 求解投影矩阵需要最少 6 对点对应
- 径向畸变：枕形、桶形 $\lambda=1 \pm \sum_{p=1}^3 k_p d^{2p}$，$k_p$ 为畸变因子

<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 单视图几何

- 无穷远点、无穷远线与无穷远平面
- 影消点和影消线
- 单视重构


<!--v-->
<!-- .slide: data-background="lec1/background.png" -->


## 无穷远点、无穷远线与无穷远平面

- 点在直线上：$x^\top l = 0$
- 直线交点：$x = l \times l^\prime$
- 点到点的变换：$p^\prime=Hp$
- 线到线的变换：$l^\prime = H^{-\top}l$
- 无穷远点：两条平行直线的交点 $l \times l^\prime = [b, -a, 0]^\top = x_\infty$，此点在欧式坐标中位于无穷远处。（透视没有无穷远点，仿射有无穷远点）
- 无穷远线：无穷远点集位于的一条直线上 $l_\infty = [0,0,1]^\top$（透视没有无穷远线，仿射有无穷远线）
- 无穷远平面
    - 平行平面在无穷远处交于一条公共线，即无穷远直线
    - 两条或多条无穷远直线的集合为无穷远平面


<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 影消点和影消线

- 影消点
    - 三维空间中的无穷远点在图像平面上的投影点
    - 影消点与直线方向：$d$ = 直线方向 = $[a,b,c]^\top$，$v$ 为影消点，则 $v=Kd$
- 影消线
    - $l_{h}=H_p^{-\top}l_\infty$
    - 图像中两条直线的交点如果在影消线上，则这两条线是 3D 空间中的平行线
    - 影消线与平面法向量：$l_{h}$ 为影消线，$n$ 为平面法向量，$K$ 为摄像机内参数，则 $n=K^\top l_h$


<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 单视重构

- 单视图恢复摄像机坐标系下的三维场景结构（场景的实际比例无法恢复）
- 单视图标定
    - 手动选择影消点影消线（利用无穷远处的点和线、面的垂直关系）
    - 需要场景先验信息

<!--v-->
<!-- .slide: data-background="lec1/background.png" -->


## 三维重建基础

- 理想情况下 $P=l \times l^\prime$，但由于噪声的存在，两条直线通常不相交
- 三角化
    - 问题：已知 $p,p^\prime,K,K^\prime,R,T$，求解 $P$ 点的三维坐标
    - 线性解法：求解超定齐次线性方程组（方程数 4 个，未知数 3 个）
    - 非线性解法：寻找 $P$ 最小化 $d(p, MP)+d(p^\prime, M^\prime P)$
    - 但实际应用中，摄像机的 $R,T$ 未知，甚至 $K,K^\prime$ 都未知
- 多视图几何的关键问题
    - 摄像机几何：从一张或者多张图像中求解摄像机的内、外参数
    - 场景几何：通过二至多幅图寻找 3D 场景坐标
    - 对应关系：已知一个图像中的 $p$ 点，如何在另外一个图像中找到 $p^\prime$ 点

<!--v-->
<!-- .slide: data-background="lec1/background.png" -->


## 极几何

- 极几何描述了同一场景或者物体的两个视点图像间的几何关系
- 极平面、基线、极线、极点
- 极几何约束：将搜索范围缩小到对应的极线上
    - E 对规范化摄像机拍摄的两个视点图像间的极几何关系进行代数描述
    - 本质矩阵 $E=T \times R = [T_\times] R$，使得 $p^{\prime \top}Ep=0$
    - F 对一般透视摄像机拍摄的两个视点图像间的极几何关系进行代数描述
    - 基础矩阵 $F=K^{\prime -\top}[T_\times]RK^{-1}$，使得 $p^{\prime \top}Fp=0$
- 基础矩阵估计：归一化八点法
- 单应矩阵：空间平面在两个摄像机下的投影几何 $H=K^\prime(R+tn_d^\top)K^{-1}$（要求场景中的点位于同一个平面，或者两个相机之间只有旋转而无平移）

<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 多视图几何

### 运动恢复结构问题
   
- 已知
    - $n$ 个三维点 $X_j(j=1,...,n)$ 在 $m$ 张图像中的对应点的像素坐标 $x_{ij}$
    - $m$ 张图像对应的摄像机的内参数矩阵 $K_i(i=1,...,m)$
    - $x_{ij}=M_iX_j=K_i[R_i \ \ T_i]X_j(i=1,...,m;j=1,...,n)$
- 求解
    - $n$ 个三维点 $X_j(j=1,...,n)$ 的坐标
    - $m$ 个摄像机的外参数 $R_i$ 及 $T_i(i=1,...,m)$


<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 多视图几何

### 三种典型的运动恢复结构任务

- 欧式结构恢复（摄像机内参数已知，外参数未知）
    1. 代数方法（求解基础矩阵、本质矩阵，分解本质矩阵）
    2. 三角化
- 仿射结构恢复（摄像机为仿射相机，内、外参数均未知）
    1. 数据中心化
    2. 因式分解获得运动与结构
- 透视结构恢复（摄像机为透视相机，内、外参数均未知）
    1. 代数方法（通过基础矩阵）
    2. 捆绑调整（Bundle Adjustment，BA）

<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 捆绑调整（Bundle Adjustment，BA）

- 恢复结构和运动的非线性方法，最小化重投影误差

$$
E(M,X)=\sum_{i=1}^m\sum_{j=1}^nD(x_{ij},M_iX_j)^2
$$

- 常用做 SFM 的最后一步，分解或代数方法可作为优化问题的初始解

<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## PnP 问题

- PnP 问题：通过世界中 N 个三维点坐标及其在图像中 N 个像点坐标，计算出相机或物体位姿的问题
- P3P：N=3 的 PnP 问题
    - 已知：摄像机内参数 K，像素平面上 a,b,c 点的像素坐标以及其对应的三维点 A,B,C 在世界坐标系中的世界坐标
    - 求解：摄像机外参数 R,T

<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 模型拟合 RANSAC

- 随机采用一致性 RANSAC：一种适用于数据收到异常值污染的模型拟合方法

1. 随机均匀采样获取模型求解所需的最小子集
2. 适用该子集估计模型参数
3. 计算剩余样本与当前模型的一致性，统计满足当前模型的点（内点）的个数，作为当前模型分数
4. 以设定的次数重复(1-3)，最终输出分数最高的模型

<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 特征提取 & 特征匹配

### 特征提取 SIFT

- 输入：图片
- 输出：具有尺度不变性的特征点（位置+每个特征的 128 维数据描述）

<hr/>

### 特征匹配

- 找到距离其最近的特征点 $j_1$ 以及次近的特征点 $j_2$，并记录 $j_1,j_2$ 与特征点 $i$ 之间的距离为 $d_1,d_2$；
- 计算距离比 $d_1/d_2$，如果小于某个阈值，则认为左图特征点 $i$ 与右图特征点 $j_1$ 是一对匹配点

<!--s-->
<!-- .slide: data-background="lec1/background.png" -->

<div class="middle center">
<div style="width: 100%">

# Part.2 SFM 基础内容

</div>
</div>

<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## SFM 系统问题描述（欧式重构）

- 已知：三维场景的 m 张图像以及每张图像对应的摄像机内参数矩阵 $K_i(i=1,...,m)$
- 求解：
    - 三维场景结构，即三维场景点坐标 $X_j(j=1,...,n)$；
    - m 个摄像机的外参数 $R_i$ 及 $T_i$(i=1,...,m)


<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## SFM 系统（两视图）

- 问题
    - $x_{1j}=M_1X_j=K_1[I \ \ \ 0]X_j (j=1,...,n)$
    - $x_{2j}=M_2X_j=K_2[R \ T]X_j (j=1,...,n)$
- 求解步骤
    1. 对应点计算（SIFT 特征提取 + 近邻匹配）
    2. 求解基础矩阵 F（RANSAC + 归一化八点法）
    3. 求解本质矩阵 $E=K_2^\top F K_1$
    4. 分解本质矩阵 $E \to R$、$T \to M_2$
    5. 三角化


<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 基于增量法的 SFM 系统（多视图）

- 求解步骤
    1. 预处理
        - 图像特征点提取与匹配
        - 基于 RANSAC 的基础矩阵或单应矩阵估计
    2. 增量法求解 SFM

<!--v-->
<!-- .slide: data-background="lec1/background.png" -->


## 增量法


<section class="increment">

<!-- <style>
.reveal p, .reveal li {
    /* font-size: 20px; */
}
</style> -->

- 输入：摄像机内参数、特征点和几何校验后的匹配结果
- 输出：三维点云、摄像机位姿


1. 计算对应点的轨迹 $(Track) \ t$
2. 计算连通图 $G$（结点代表图片，边代表其之间有足够的匹配点）
3. 在 $G$ 中选取一条边 $e$
4. 鲁棒估计 $e$ 所对应的本质矩阵 $E$
5. 分解本质矩阵 $E$，得到两张图片摄像机的位姿（外参数）
6. 三角化 $t \cap e$ 的点，作为初始化的重建结果
7. 删除 $G$ 中的边 $e$
8. 如果 $G$ 中还有边：
    1. 从 $G$ 中选取 $e$ 满足 $track(e) \cap $已重建 3D 点 最大化
    2. 用 PnP 方法估计摄像机位姿（外参数）
    3. 三角化新的 $tracks$
    4. 删除 $G$ 中的边 $e$
    5. 执行 Bundle Adjustment
9. 结束

</section>

<!--s-->
<!-- .slide: data-background="lec1/background.png" -->

<div class="middle center">
<div style="width: 100%">

# Part.3 SFM 代码复现


</div>
</div>

<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 运行环境

- 虚拟机 Ubuntu 22.04 Arm64
- openMVG+PMVS


<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 遇到的错误

- 错误

```CMake
/home/jujimeizuo/Workspace/openMVG/src/openMVG/system/cpu_instruction_set.hpp:18:12: fatal error: cpuid.h: No such file or directory
   18 |   #include <cpuid.h>
      |            ^~~~~~~~~
compilation terminated.
```

- 错误原因：在非 x86-64 架构的机器上，`cpuid.h` 文件不存在
- 解决方法：注释 cpuid.h 那一行和有关 `internal_cpuid` 函数的代码，并直接返回 `false`

<!--v-->
<!-- .slide: data-background="lec1/background.png" -->

## 三维重建实例（城堡）

1. OpenMVG提取稀疏点云

```bash
cd openMVG_Build/software/SfM/tutorial_demo.py
python tutorial_demo.py
```

2. PMVS重建稠密点云、重建表面和纹理映射过程

```bash
# 把openMVG生成的SfM_Data转为适用于PMVS输入格式的文件
cd tutorial_out/reconstruction_global/
openMVG_main_openMVG2PMVS -i sfm_data.bin -o ./
# 使用PMVS重建稠密点云、表面、纹理
pmvs2 ./PMVS/ pmvs_options.txt
```

3. 用 MeshLab 查看生成的点云

<!--v-->

## 稀疏点云（城堡）

<center><img src="lec1/sparse-point-cloud.jpg" alt="稠密点云"></center>

<!--v-->

## 稠密点云（城堡）

<center><img src="lec1/dense-point-cloud.jpg" alt="稠密点云"></center>

<!--s-->
<!-- .slide: data-background="lec1/background.png" -->

## Reference

- 多视图几何
- [计算机视觉之三维重建（深入浅出SfM与SLAM核心算法）](https://www.bilibili.com/video/BV1DQ4y1e7x6/?spm_id_from=333.788&vd_source=5e048b202705330980eefcc9a56cc5d0)
- [使用openMVG+PMVS实现视觉三维重建](https://blog.yanjingang.com/?p=3329)

<!--s-->
<!-- .slide: data-background="lec1/background.png" -->


<div class="middle center">
<div style="width: 100%">

# 谢谢大家

<hr/>

**Question?**

</div>
</div>