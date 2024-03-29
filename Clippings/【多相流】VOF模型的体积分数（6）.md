# 【多相流】VOF模型的体积分数（6）
[【多相流】VOF模型的体积分数（6）](https://www.zhihu.com/tardis/zm/art/161299032?source_id=1005) 

 通过求解一个(或多个)相的体积分数连续性方程来实现对两相之间界面的跟踪。对第q相，这个方程有以下形式:  

![](https://pic3.zhimg.com/v2-5134dba90829a94e8433a342b4d4f336_b.webp?consumer=ZHI_MENG)

其中m_qp为q相到p相的传质，m_pq为p相到q相的传质。默认情况下，方程18.8右边的源项是零，但是你可以为每个相指定一个常数或用户定义的质量源。关于多相流模型传质，在后面质量传递那一节做详细的讨论。

主相的体积分数方程不求解;主相体积分数的计算将基于以下约束条件:

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-6-23%2021-41-56/59c324cd-a28c-4295-84b6-7c9c5e3fe653.webp?raw=true)

体积分数方程可以用隐式或显式时间公式求解。

**1 隐式公式**
----------

采用隐式公式时，将体积分数方程离散如下:

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-6-23%2021-41-56/c0a81e77-2d32-4a76-a250-f8649b875ad0.webp?raw=true)

当前时间步长的体积分数是当前时间步的其他量的函数，对每个时间步的次相体积分数的标量传输方程进行迭代求解。

采用所选的空间离散方案对面通量进行插值。隐式公式可用于瞬态和稳态计算。

**2 显式公式**
----------

显式公式是时间相关的，体积分数离散方式如下:

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-6-23%2021-41-56/089de2d8-2ba7-411d-95a6-c1d5a3cc68c7.webp?raw=true)

由于当前时间步长的体积分数是根据前一个时间步长的已知量直接计算的，因此显式公式不需要在每个时间步长的传输方程进行迭代求解。

利用界面跟踪或捕获方法，如Geo-Reconstruct, CICSAM, Compressive, and Modified HRIC ，可以实现面通量的插值。

ANSYS Fluent自动细化了体积分数方程积分的时间步长，但可以通过修改Courant number来影响时间步长计算。可以选择为每个时间步更新一次体积分数，或者为每个时间步中的每个迭代更新一次体积分数。

_重要提示：当使用显式格式时，必须计算瞬态。_

**3 界面附近插值**
------------

Fluent控制体积公式要求计算通过控制体积面对流和扩散的通量，并与控制体积内的源项进行平衡。

在Geo-Reconstruct和donor-acceptor方案中，Fluent对两相界面附近的单元进行了特殊的插值处理。图18.2:界面的计算给出了一个实际的界面形状以及假定的计算方法的界面。

![](https://pic4.zhimg.com/v2-2f323db13703a47090aa8c8f7b847d97_b.webp?consumer=ZHI_MENG)

显式方案和隐式方案以相同的插值处理这些单元，因为单元格完全充满了一个相或另一个相，而不是应用特殊处理。

**3.1 Geometric Reconstruction方法**
----------------------------------

在几何重构方法中，利用Fluent中的标准插值格式，在单元格完全填充某一相或另一相位，计算出面的通量。当单元靠近两相之间的界面时，采用几何重建方案。

几何重构方法用分段逼近的方法表示流体间的界面。在Fluent中，该方案是最精确的，适用于一般的非结构网格。根据Youngs的工作，推广了非结构网格的几何重建方案。它假设两种流体之间的界面在每个单元内具有一个线性斜率，并利用这个线性形状来计算流体通过单元面的对流。

这个重构方案的第一步是根据体积分数及其在单元中的导数信息，计算线性界面相对于每个填充部分单元中心的位置。第二步是利用计算得到的线性界面表示法向和切向速度分布信息，计算各面流体的对流量。第三步是使用前一步计算的通量平衡计算每个单元的体积分数。

**重要提示**：当使用几何重建方案时，必须计算一个时变的解。此外，如果使用共形网格(也就是说，如果网格节点的位置在两个子域相交的边界上是相同的)，则必须确保域内没有双面(零厚度)。如果有，你需要切开它们。

**3.2 Donor-Acceptor**
----------------------

在donor-acceptor方法中，使用Fluent中使用的标准插值格式来获得当单元完全充满某一相位或另一相位时的面通量。当单元靠近两相之间的界面时，使用“donor-acceptor”方法来确定流过面的通量。该方法将一个单元标识为来自某一相的一定量的流体的供体，而另一个(邻近的)单元标识为相同数量流体的受体，并用于防止界面上的数值扩散。来自一个相的流体可以通过一个单元边界进行对流，其数量受两个值的限制:供体单元的填充体积或受体单元的自由体积。

界面的方向也用于确定面通量。界面的方向要么是水平的，要么是垂直的，这取决于单元内q相位的体积分数梯度的方向，以及相邻单元共享的面。根据界面的方向及其运动，通量值可以通过单独的顺风、单独的逆风或两者的某种组合获得。当使用donor-acceptor方案时，必须计算一个依赖时间的解。同时，该方案只能用于四边形或六面体网格。此外，如果使用共形网格(也就是说，如果网格节点位置在两个子域相交的边界上是相同的)，则必须确保域内没有双面(厚度为零)。

**3.3Compressive Interface Capturing Scheme for Arbitrary Meshes (CICSAM)**
---------------------------------------------------------------------------

CICSAM是一种基于Ubbink的工作的高分辨率差分方法。CICSAM方法特别适用于相间粘度比高的流动。CICSAM作为一种显式方法在Fluent中实现，其优点是生成的界面几乎与几何重构方法一样清晰。

**3.4 The Compressive Scheme and Interface-Model-based Variants**
-----------------------------------------------------------------

该压缩方案是一种基于斜率限制的二阶重构方案。在空间离散格式中使用斜率限制是为了避免高阶空间离散格式由于解域的剧烈变化而产生的伪振荡或摆动。下面的理论适用于采用压缩方法结构的区域离散化和相的局部离散化。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-6-23%2021-41-56/d7ba1776-ed87-46d3-abd1-a336e909dc9c.webp?raw=true)

斜率限制器限制在0到2之间(包括2)。对于小于1的值，空间离散化用低分辨率方案表示。对于1到2之间的值，空间离散化用高分辨率方案表示。

压缩格式离散化取决于界面状态类型的选择。选择锐化界面状态建模，压缩方案只适用于锐化界面建模。然而，当选择锐/分散界面建模时，压缩方案适用于锐/分散界面建模。

**3.5 Bounded Gradient Maximization (BGM)**
-------------------------------------------

在VOF模型中引入了BGM方案来获得清晰的界面，与几何重构方案相比具有较好的优势。目前，该方案仅适用于稳态求解器，不适用于瞬态问题。在BGM方案中，离散化以这样一种方式发生，即通过使面值向向外推顺风值加权的程度最大化，从而使梯度的局部值最大化。

编辑于 2020-07-17 · 著作权归作者所有