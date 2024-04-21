> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/693665318)

一、算法原理
------

基于曲面拟合的方法，实现对点云的平滑处理。

### 1、主要函数

**头文件**

```
#include <CGAL/jet_smooth_point_set.h>
```

**函数**

```
void CGAL::jet_smooth_point_set  ( PointRange &  points,  
  unsigned int  k,  
  const NamedParameters &  np = parameters::default_values()  
 )
```

使用射流拟合在最近的邻居和重新投影到射流上的点的范围平滑。由于此方法会改变点的位置，因此不适用于进行过排序处理的点云。

*   points：点云
*   k： 隐式曲面拟合的邻域点数。值越大，结果越平滑。
*   np：下面列出的命名参数中的一个可选序列。

![](https://pic4.zhimg.com/v2-760bea397d95911acbfd13fda8eec53f_b.jpg)

二、代码实现
------

```
#include <vector>
#include <iostream>    // io
#include <pcl/io/pcd_io.h>          // PCL读取PCD
#include <pcl/point_types.h>        // PCL点类型
#include <CGAL/Point_set_3/IO/XYZ.h>// CGAL保存点为.xyz

#include <CGAL/jet_smooth_point_set.h>
#include <CGAL/Simple_cartesian.h>

typedef CGAL::Simple_cartesian<float> Kernel;

int main()
{
	//-------------------------加载PCD点云数据------------------------------
	pcl::PointCloud<pcl::PointXYZ>::Ptr cloud(new pcl::PointCloud<pcl::PointXYZ>);

	if (pcl::io::loadPCDFile<pcl::PointXYZ>("cgal//cloud.pcd", *cloud) == -1)
	{
		PCL_ERROR("Could not read file\n");
		return -1;
	}
	// ----------------------转为CGAL支持的格式------------------------------
	std::vector<Kernel::Point_3> points;
	for (size_t i = 0; i < cloud->size(); ++i)
	{
		float px = cloud->points[i].x;
		float py = cloud->points[i].y;
		float pz = cloud->points[i].z;
		points.push_back(Kernel::Point_3(px, py, pz));
	}

	// ---------------------------Smoothing----------------------------------
	const unsigned int nb_neighbors = 24; // 邻域点的个数
	CGAL::jet_smooth_point_set<CGAL::Parallel_if_available_tag>(points, nb_neighbors);
	//CGAL::IO::write_XYZ("cgal//smooth.xyz", points);

	pcl::PointCloud<pcl::PointXYZ>::Ptr smoothed(new pcl::PointCloud<pcl::PointXYZ>);
	smoothed->resize(points.size());
	smoothed->reserve(points.size());

	for (size_t i = 0; i < points.size(); ++i)
	{
		smoothed->points[i].x = points[i].hx();
		smoothed->points[i].y = points[i].hy();
		smoothed->points[i].z = points[i].hz();
	}
	pcl::io::savePCDFileBinary("cgal//jet_smooth.pcd", *smoothed); 
	
	return 0;
}
```

三、结果展示
------

![](https://pic4.zhimg.com/v2-3f4231741849d07705a5939241e9f00f_r.jpg)