# Understanding vtkPolyData: The Basics - Dillon Huff
[Understanding vtkPolyData: The Basics - Dillon Huff](http://www.dillonbhuff.com/?p=540) 

 Understanding vtkPolyData: The Basics
=====================================

March 29, 2017 [dillonhuff](http://www.dillonbhuff.com/?author=1 "Posts by dillonhuff") [Leave a comment](http://www.dillonbhuff.com/?p=540#respond)

A vtkPolyData is just a list of points in 3D space along with lists of lines, polygons, triangles, and other shapes defined on those points. That is a pretty abstract description so lets look at an example of how we could represent some shapes as a vtkPolyData. Consider the picture below:

_**[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-12-21%2018-15-58/79038342-cc64-48fa-aba0-4d062b3c2004.jpeg?raw=true)
](http://www.dillonbhuff.com/wp-content/uploads/2017/03/vtkPolyDataShapes.jpg)**_

This group of shapes consists of a triangle and 4 lines. If this arrangement were stored in a vtkPolyData it would be stored as 3 lists like so:

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-12-21%2018-15-58/d2f4eccb-145f-4e79-9d28-2de84dc94475.jpeg?raw=true)
](http://www.dillonbhuff.com/wp-content/uploads/2017/03/vtkPolyDataPDPic-1.jpg)

Each of the three lists shown above (points, lines, and polygons) correspond to private members of vtkPolyData. All the points needed to define all the shapes that make up the vtkPolyData are stored in the **points** buffer. Then lines and polygons are stored as lists of references to points in the buffer.

For example the first element in the lines list is {0, 1}, which represents the line between (0, 0, 0) (the 0th element of **points**) and (1, 0, 0) (the 1th element of **points**).

In the language of VTK the **points** array is an instance of the[vtkPoints](http://www.vtk.org/doc/nightly/html/classvtkPoints.html) class, and the **lines** and **polygons** arrays are instances of the [vtkCellArray](http://www.vtk.org/doc/nightly/html/classvtkCellArray.html) class. Here is the code used to create the vtkPolyData for our group of shapes:

```
#include <vtkTriangle.h>
#include <vtkSmartPointer.h>
#include <vtkPoints.h>
#include <vtkLine.h>
#include <vtkCellArray.h>
#include <vtkPolyData.h>
 
using namespace std;
 
int main() {
 
  // Set up the array of points
  vtkSmartPointer<vtkPoints> points =
    vtkSmartPointer<vtkPoints>::New();
 
  vtkIdType p0_0_0 =
    points->InsertNextPoint(0, 0, 0);
 
  cout << "p0_0_0 = " << p0_0_0 << endl;
 
  vtkIdType p1_0_0 =
    points->InsertNextPoint(1, 0, 0);
 
  cout << "p1_0_0 = " << p1_0_0 << endl;
 
  vtkIdType p1_1_0 =
    points->InsertNextPoint(1, 1, 0);
 
  cout << "p1_1_0 = " << p1_1_0 << endl;
   
  vtkIdType p0_1_0 =
    points->InsertNextPoint(0, 1, 0);
 
  cout << "p0_1_0 = " << p0_1_0 << endl;
 
  // Create cells for lines
  vtkSmartPointer<vtkLine> l01 =
    vtkSmartPointer<vtkLine>::New();
  l01->GetPointIds()->SetId(0, p0_0_0);
  l01->GetPointIds()->SetId(1, p1_0_0);
 
  vtkSmartPointer<vtkLine> l13 =
    vtkSmartPointer<vtkLine>::New();
  l13->GetPointIds()->SetId(0, p1_0_0);
  l13->GetPointIds()->SetId(1, p0_1_0);
 
  vtkSmartPointer<vtkLine> l30 =
    vtkSmartPointer<vtkLine>::New();
  l30->GetPointIds()->SetId(0, p0_1_0);
  l30->GetPointIds()->SetId(1, p0_0_0);
 
  vtkSmartPointer<vtkLine> l23 =
    vtkSmartPointer<vtkLine>::New();
  l23->GetPointIds()->SetId(0, p1_1_0);
  l23->GetPointIds()->SetId(1, p0_1_0);
   
  // Add the lines to the lines cell array
  vtkSmartPointer<vtkCellArray> lines =
    vtkSmartPointer<vtkCellArray>::New();
 
  lines->InsertNextCell(l01);
  lines->InsertNextCell(l13);
  lines->InsertNextCell(l30);
  lines->InsertNextCell(l23);
 
  // Create the triangle
  vtkSmartPointer<vtkTriangle> triangle =
    vtkSmartPointer<vtkTriangle>::New();
  triangle->GetPointIds()->SetId(0, p0_0_0);
  triangle->GetPointIds()->SetId(1, p1_0_0);
  triangle->GetPointIds()->SetId(2, p0_1_0);
   
  // Add the triangle to polygons
  vtkSmartPointer<vtkCellArray> polygons =
    vtkSmartPointer<vtkCellArray>::New();
 
  polygons->InsertNextCell(triangle);
   
  // Create the actual vtkPolyData
  vtkSmartPointer<vtkPolyData> polyData =
    vtkSmartPointer<vtkPolyData>::New();
 
  // Add the geometry and topology to the polydata
  polyData->SetPoints(points);
  polyData->SetLines(lines);
  polyData->SetPolys(polygons);
 
  // Print out some data about the vtkPolydata
  cout << "# of lines    = " << polyData->GetNumberOfLines() << endl;
  cout << "# of polygons = " << polyData->GetNumberOfPolys() << endl;
   
}
```

The full example is available [here](https://github.com/dillonhuff/vtkpolydata_basics).

What is going on in this code snippet?
======================================

Really we are just building the three lists shown in the picture above and then wrapping a vtkPolyData around them.

First we create the points list. The important thing to note is that as we add points to the vtkPoints data structure it returns the handle (really just an offset) that will let us refer to the location of the point in the vtkPoints list.

Next we build the lists of lines and polygons using the handles for points. These lists have the very odd name vtkCellArray. vtkCell is a very general base class from which things like lines, polygons, hexadrons, and voxels are derived. For our purposes vtkCellArray could be called vtkShapeArray. It is just a list of shapes.

Posted in: [Uncategorized](http://www.dillonbhuff.com/?cat=1)