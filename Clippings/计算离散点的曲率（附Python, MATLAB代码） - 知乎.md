# 计算离散点的曲率（附Python, MATLAB代码） - 知乎
[计算离散点的曲率（附Python, MATLAB代码） - 知乎](https://zhuanlan.zhihu.com/p/72083902) 

 在很多学科中的很多计算任务中都需要用到曲线的曲率（或者曲率半径），numpy库里和matlab build-in里都没有现成的能从离散点来算曲率的方法，网上找到的代码又不敢直接用，毕竟是要高频率用到自己科研上的工具，所以决定结合找到的资料自己推一下，并造出python和matlab的轮子，造福后人

公式很简单：

曲率：

$$\\kappa = \\frac{|\\ddot{\\vec{r}}\\times\\dot{\\vec{r}}|}{\\left| \\dot{\\vec{r}}\\right|^3}$$

在二维情况下，其标量形式为：

κ=|x″y′−x′y″|((x′)2+(y′)2)3/2\\kappa = \\frac{|x''y'-x'y''|}{\\left( (x')^2+(y')^2\\right)^{3/2}}

所以对于解析情况非常简单，可以直接对于曲线表达式进行解析求导，但是对于离散的点，情况反倒会比较复杂，因为这里的x和y的一阶和二阶导数如果直接用差分方法来计算的话会造成比较大的误差。

这里比较保险的做法是，使用三个点确定的二次曲线的的曲率作为我们估计的曲率:

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-4-17%2014-11-50/064d4ef2-90bd-4dab-a66f-5f52ee0c43e3.jpeg?raw=true)

然后使用中间这个点(x2,y2)处的曲率作为这三个点的曲率估计

具体方法是先表示成参数方程的样子，

对于曲线参数t，有：

{x=a1+a2t+a3t2y=b1+b2t+b3t2\\left\\{ \\begin{matrix} x = a\_1 +a\_2 t+a\_3 t^2\\\ y= b\_1 +b\_2 t+b\_3 t^2 \\end{matrix} \\right.

6个未知数，三个点里有6个已知分量，列六个方程，解出这 (a1,a2,a3), (b1,b2,b3)即可。

这里使用两段矢量的长度来作为取值范围：

ta=(x2−x1)2+(y2−y1)2tb=(x3−x2)2+(y3−y2)2t\_a = \\sqrt{(x\_2-x\_1)^2 + (y\_2-y\_1)^2}\\\ t\_b = \\sqrt{(x\_3-x\_2)^2 + (y\_3-y\_2)^2}

这里我们希望参数方程中的t满足如下条件：

(x,y)|t=−ta=(x1,y1)(x,y)|t=0=(x2,y2)(x,y)|t=tb=(x3,y3)(x,y)|_{t=-t\_a} = (x\_1,y\_1)\\\ (x,y)|\_{t=0} = (x\_2,y\_2)\\\ (x,y)|_{t=t\_b} = (x\_3,y_3)

则有：

{x1=a1−a2ta+a3ta2x2=a1x3=a1+a2tb+a3tb2\\left\\{ \\begin{matrix} x\_1 &= & a\_1 &- a\_2 t\_a +a\_3 t\_a^2\\\ x\_2 &= &a\_1 &\\\ x\_3 &= &a\_1 &+ a\_2 t\_b + a\_3 t\_b^2 \\end{matrix} \\right.

以及

{y1=b1−b2ta+b3ta2y2=b1y3=b1+b2tb+b3tb2\\left\\{ \\begin{matrix} y\_1 &= & b\_1 &- b\_2 t\_a + b\_3 t\_a^2\\\ y\_2 &= &b\_1 &\\\ y\_3 &= &b\_1 &+ b\_2 t\_b + b\_3 t\_b^2 \\end{matrix} \\right.

写成矩阵形式：

(x1x2x3)=(1−tata21001tbtb2)(a1a2a3)\\left( \\begin{matrix} x\_1\\\ x\_2\\\ x\_3\\\ \\end{matrix} \\right) = \\left( \\begin{matrix} 1& -t\_a &t\_a^2\\\ 1& 0 & 0\\\ 1& t\_b& t\_b^2\\\ \\end{matrix} \\right) \\left( \\begin{matrix} a\_1\\\ a\_2\\\ a\_3\\\ \\end{matrix} \\right)

以及

(y1y2y3)=(1−tata21001tbtb2)(b1b2b3)\\left( \\begin{matrix} y\_1\\\ y\_2\\\ y\_3\\\ \\end{matrix} \\right) = \\left( \\begin{matrix} 1& -t\_a &t\_a^2\\\ 1& 0 & 0\\\ 1& t\_b& t\_b^2\\\ \\end{matrix} \\right) \\left( \\begin{matrix} b\_1\\\ b\_2\\\ b\_3\\\ \\end{matrix} \\right)

简写为：

X=MAY=MBX=M A\\\ Y=M B

可以使用求矩阵逆的方式求解线性方程:

A=M−1XB=M−1YA = M^{-1}X\\\ B = M^{-1}Y

有了(a1,a2,a3)和(b1,b2,b3)就有了曲线的解析方程，接下来就和解析求曲率一样了，先算变量导数：

x′=dxdt|t=0=a2x″=d2xdt2|t=0=2a3y′=dydt|t=0=b2y″=d2ydt2|t=0=2b3x'= \\left.\\frac{dx}{dt}\\right|_{t=0} = a\_2\\\ x''= \\left.\\frac{d^2x}{dt^2}\\right|\_{t=0} =2 a\_3\\\ y'= \\left.\\frac{dy}{dt}\\right|\_{t=0} = b\_2\\\ y''= \\left.\\frac{d^2y}{dt^2}\\right|\_{t=0} =2 b_3

然后就是最终的曲率：

κ=x″y′−x′y″((x′)2+(y′)2)3/2=2(a3b2−a2b3)(a22+b22)3/2\\kappa = \\frac{x''y'-x'y''}{\\left( (x')^2+(y')^2\\right)^{3/2}} = \\frac{2(a\_3 b\_2-a\_2 b\_3)}{(a\_2^2+b\_2^2)^{3/2}}

这样，任意给定三个点，都可以估计出这三个点是比较【弯的】还是比较【直的】，直的曲率小，van的曲率大。

随手生成两个例子算算：

（1）圆形：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-4-17%2014-11-50/9be48c59-1c6a-4613-ab5c-511ebc2c5946.jpeg?raw=true)

好理解，圆形所有地方都是一样弯

（2）正弦

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-4-17%2014-11-50/42e8594e-3a9b-4226-a9f8-60a2833bec6a.jpeg?raw=true)

波峰和波谷最弯，0点附近最直。

看起来没毛病，核对了方向和曲率半径大小的数值，都能对上。这是一个可以放心使用的轮子

这里我把这个算法实现为Python和MATLAB，详见github：

引用方式：

[\[Cite\] Pjer-zhang/PJCurvature](https://link.zhihu.com/?target=https%3A//github.com/Pjer-zhang/PJCurvature/blob/master/cite.md)

  
===================

Pjer内容分类整理：

[精选](https://www.zhihu.com/collection/334151662) [射电](https://www.zhihu.com/collection/334150369) [编程](https://www.zhihu.com/collection/334149416) [科研工具](https://www.zhihu.com/collection/334151416) [太阳物理](https://www.zhihu.com/collection/334150296)
