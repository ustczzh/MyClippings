> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/455044099)

**背景**
------

在 CFD 计算中，源项的求解需要用到网格体积，比如在湍动能的输运方程中：

$$\frac{\partial(\rho k)}{\partial t}+\nabla \cdot(\rho k \boldsymbol{U})=\nabla \cdot\left(\left(\mu+\frac{\mu_{t}}{\sigma_{k}}\right) \nabla k\right)+P_{\epsilon}-\rho \epsilon+S_{k} \\$$

将湍动能在网格进行积分，可以得到：

$$\int_{V}\left[\frac{\partial(\rho k)}{\partial t}+\nabla \cdot(\rho k U)-\nabla \cdot\left(\left(\mu+\frac{\mu_{t}}{\sigma_{k}}\right) \nabla k\right)-P_{c}+\rho e-S_{k}\right] \mathrm{d} V=0 \\$$

篇幅所限，这里只讨论最后一项源项，在使用有限体积法求解输运方程时，首先需要对控制体积上的方程进行积分，由于积分是满足加减法运算，因此可以对每一项单独积分再相加，因此可以得到源项的对控制体积的积分：

$$\int_{V}\left[S_{k}\right] d V \\$$

因此，源项的体积积分可以等于网格中心的值乘上网格体积：

$$\int_{V}\left[S_{k}\right] \mathrm{d} V=S_{p} V_{p} \\$$

在 CFD 计算中，所有变量在网格中都是线性变化。如果源项在网格中是线性变化的，则可以通过中值定理来求解。

> **中值定理**  
> 在网格中任意的两个节点之间画一条线，可以发现变量在网格中呈线性变化，就是下图红色实线所示，如果我们积分这条线下的面积，无论直线的斜率如何变化，积分的效果都是一样。

![](https://pic1.zhimg.com/v2-3e1660f2e366b2f84edc259d0c3f7478_b.png)

因此，源项的体积积分可以等于网格中心的值乘上网格体积：

$$\int_{V}\left[S_{k}\right] \mathrm{d} V=S_{p} V_{p} \\$$

在 CFD 计算中，所有变量在网格中都是线性变化。如果源项在网格中是线性变化的，则可以通过中值定理来求解。

那么网格体积$V_{p}$ 如何求解呢？

**网格体积**
--------

对于简单的网格，比如直角三角形、正方形或其他四边形，可以通过简单的方法（比如一些基本的向量微积分）来计算网格的面积或体积。

但在实际划分网格时，网格通常不是规则的方形或三角形，更多的是各种不规则的形状。

![](https://pic2.zhimg.com/v2-d9b6edc0dc93d88fe9572c6b0e5bdd61_r.jpg)

我们需要一种方法可以计算这些网格体积，包括多面体网格、倾斜网格和拥有不同尺寸面的网格。

本文从**散度理论**推导网格体积的计算方法，这种方法可以应用于任何形状的网格体积计算。

**散度定理**
--------

对于任意向量场$\boldsymbol{B}$，高斯散度定理均可以表示为：

$$\int_{V}(\nabla \cdot \boldsymbol{B}) \mathrm{d} V=\int_{A}(\boldsymbol{B} \cdot \hat{\boldsymbol{n}}) \mathrm{d} A \\$$

为了可以通俗地理解，想象有一个盒子，向量场$\boldsymbol{B}$ 用盒子里面的小球表示：

![](https://pic4.zhimg.com/v2-6ff3032f52ce1dade5cc7e16c2f28dc3_r.jpg)

盒子里面累积小球的数量等于小球通过盒子外表面的通量，这就是散度定理的全部含义。

不难发现公式的左边是体积积分，右边是面积分，现在我们要利用这一点计算网格体积。

令$\nabla \cdot B=1$，可以得到关于网格体积的方程：

$$\begin{gathered} \int_{V}(1) \mathrm{d} V=\int_{A}(\boldsymbol{B} \cdot \hat{n}) \mathrm{d} A \\ V_{p}=\int_{A}(\boldsymbol{B} \cdot \hat{\boldsymbol{n}}) \mathrm{d} A \end{gathered} \\$$

**网格体积求解**
----------

散度定理对任意场$\boldsymbol{B}$ 都是有效的，我们需要找到一个场满足$\nabla \cdot B=1$，以便求解网格体积。

令$B=\frac{1}{3}(x, y, z)$，则：

$$\begin{aligned} \nabla \cdot B &=\frac{1}{3}(x, y, z) \cdot\left(\frac{\partial}{\partial x}, \frac{\partial}{\partial y}, \frac{\partial}{\partial z}\right) \\ &=\frac{1}{3}(1+1+1) \\ &=1 \end{aligned} \\$$

将上式带入散度定理：

$$\int_{V}(\nabla \cdot B) \mathrm{d} V=\int_{A}(B \cdot \hat{n}) \mathrm{d} A \\$$

就可以得到：

$$\begin{gathered} V_{P}=\int_{A}(B \cdot \hat{n}) \mathrm{d} A \\ V_{P}=\int_{A}\left(\frac{1}{3}(x, y, z) \cdot\left(n_{x}, n_{y}, n_{z}\right)\right) \mathrm{d} A \end{gathered} \\$$

这是一个对网格所有表面的面积分。对于网格表面所有的点，都有其对应的法向向量，以下图为例（当然网格的性质可以是任意）。

![](https://pic3.zhimg.com/v2-44369bf39eee762aea06c0af8b1db23e_b.jpg)

方程的含义就是在网格表面积分单位法向向量与其点左边的乘积。但是在曲面求解这个积分同样存在困难，需要进一步简化。

```
高斯散度定理对所有封闭空间均是有效的，无论是规则或者不规则的形状
```

每个网格的面是有限的，我们可以把整个曲面的积分，分解为每个面的曲面积分，在累加起来，就可以得到：

$$V_{P}=\sum_{\text {Faces }} \int_{A}\left(\frac{1}{3}(x, y, z) \cdot\left(n_{x}, n_{y}, n_{z}\right)\right) \mathrm{d} A \\$$

前面我们提到所有变量在网格中的变化都是线性的，如果下图蓝色阴影渐变一般。同样变量在网格表面变化也是线性的。因此我们可以用面中心的值代替整个面的积分。

![](https://pic1.zhimg.com/v2-57bfcb13c928ad8ad1a272489420cbc8_b.jpg)

因此方程中的面积分可以转换为：

$$\begin{gathered} V_{P}=\sum_{\text {Faces }}\left(\frac{1}{3}\left(x_{f}, y_{f}, z_{f}\right) \cdot\left(n_{x}, n_{y}, n_{z}\right)\right) A_{f} \\ V_{P}=\sum_{\text {Faces }}\left(\frac{1}{3}\left(x_{f} \cdot \hat{n}_{f}\right)\right) A_{f} \end{gathered} \\$$

下标$f$

表示面中心的值，这个方程就是求解网格体积的公式，并且它对所有形状的网格均是有效的。

需要特别指出的是$x_{f}, y_{f}, z_{f}$ 的含义，以$x_{f}$ 为例，如下图：

![](https://pic4.zhimg.com/v2-1f214bf005cb929b9f3048820ddcdbc3_b.jpg)

$x_{f}$ 是相对于网格中心进行测量得到，而不是建立在全局参考系的$x$ 坐标。

**2D 网格 "体积"**
--------------

公式在计算 2D 网格时会有些不同，因为它没有 "Z" 坐标。

同样基于散度定律：

$$\int_{V}(\nabla \cdot B) \mathrm{d} V=\int_{A}(B \cdot \hat{n}) \mathrm{d} A \\$$

需要找到二维向量场来满足$\nabla \cdot B=1$：

$$\left(\frac{\partial}{\partial x}, \frac{\partial}{\partial y}\right) \cdot\left(B_{x}, B_{y}\right)=1 \\$$

令$\boldsymbol{B}=\frac{1}{2}(x, y)$，可以得到：

$$\begin{aligned} \nabla \cdot \boldsymbol{B} &=\left(\frac{\partial}{\partial x}, \frac{\partial}{\partial y}\right) \cdot \frac{1}{2}(x, y) \\ &=\frac{1}{2}(1+1) \\ &=1 \end{aligned} \\$$

因此得到的 2D 计算公式略有差异：

$$\begin{aligned} &V_{P}=\sum_{\text {Faces }}\left(\frac{1}{2}\left(x_{f} \cdot \hat{n}_{f}\right)\right) A_{f} \\ &V_{P}=\sum_{\text {Faces }}\left(\frac{1}{3}\left(x_{f} \cdot \hat{n}_{f}\right)\right) A_{f} \end{aligned} \\$$

上方是二维计算方式，下方是 3 维计算方式，差别在 1/2 和 1/3。

**应用实例**
--------

考虑两个并排的矩形，长宽分别为 2、1。

![](https://pic1.zhimg.com/v2-22d3358ccc42420d0f83cbfb3feda09c_b.jpg)

接下来通过公式计算两个矩形的面积（已经知道结果为 2）。

首先关注左边的矩形：

![](https://pic1.zhimg.com/v2-95987eef038dad845cdd92fe64249890_b.jpg)

$$\begin{gathered} V_{p}=\underbrace{\frac{1}{2}(0,-0.5) \cdot(0,-1) 2}_{\text {Bottom }}+\underbrace{\frac{1}{2}(1,0) \cdot(1,0) 1}_{\text {Right }}+\underbrace{\frac{1}{2}(0,0,5) \cdot(0,1) 2}_{\text {Top }}+\underbrace{\frac{1}{2}(-1,1) \cdot(0,-1) 1}_{\text {Left }} \\ V_{p}=1 / 2+1 / 2+1 / 2+1 / 2=2 \end{gathered} \\$$

要注意到：单位法向量总是指向网格外侧，因此点积的结果总是正的。因此无论网格质量如何，其体积都是正的。

接下来看一下右侧的网格：

![](https://pic1.zhimg.com/v2-ee6e1faea16f96715be7babd10b5ac98_b.jpg)

右侧网格体积的计算方法与左侧计算方法相似：

$$V_{p}=1 / 2+1 / 2+1 / 2+1 / 2=2 \\$$

需要注意的是此处$x_{f}$ 是根据网格中心进行测量的。

**小结**
------

1. 高斯散度定理可以推导出网格体积公式；

2. 式可用于任意$N$ 面单元格 (多面体)；

3. 用于倾斜细胞和不同大小的细胞 (不规则多面体)。

From：Fluid Mechanics101