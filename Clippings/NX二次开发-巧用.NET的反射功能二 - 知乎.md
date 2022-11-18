# NX二次开发-巧用.NET的反射功能二 - 知乎
作者：谭弘利 审校：凌俊

适用版本：NX 7.0及以上版本

一、概述

反射是.NET中的重要机制，通过反射，可以在运行时动态获得程序或程序集中每一个类型（包括类、结构、委托、接口和枚举等）的成员和成员的信息。有了反射，即可对每一个类型了如指掌。另外可以直接创建对象，即使这个对象的类型在编译时还不知道；也可以直接动态调用公开或非公开的任何函数、属性和字段值。

二、详细说明

1.动态读取NX表达式（Expression）的值设置到.NET对象实例的属性（Property），通过使用特性标记标记类的属性（Property），在运行时自动读取NX表达式（Expression）的值，赋值给.NET对象实例的（Property）。

/// <summary>

/// 从部件的表达式中加载数据，然后设置到目标的属性(Property)中 。

/// </summary>

/// <param name="target">目标对象实例.</param>

/// <param name="part">部件.</param>

/// <param name="expNumber">表达式后缀.</param>

/// <exception cref="System.ArgumentNullException"> target 或 part 为空 </exception>

/// <remarks> 只有使用ExpressionAttributeAttribute作为特性标记的属性（Property）才会被赋值。 </remarks>

public static void PropertyFromExpression(this object target, NXOpen.Part part, string expNumber = "")

{

//验证输入参数是否为空

if (target == null)

{

throw new System.ArgumentNullException(nameof(target));

}

throw new System.ArgumentNullException(nameof(target));

}

if (part == null)

{

throw new System.ArgumentNullException(nameof(part));

}

//遍历实例对象的Property属性

foreach (var propertyInfo in target.GetType().GetProperties().Where(p => p.CanWrite))

{

//查找特性标记

var expAttribute = propertyInfo

.GetCustomAttributes(true)

.OfType<ExpressionAttributeAttribute>()

.FirstOrDefault(p => p.CanRead);

if (expAttribute == null)

{

continue;

}

//查找表达式的值

var value = part.GetExpression($"{expAttribute.ReadName}{expNumber}");

if (value == null)

{

continue;

}

//将值赋值到实例对象的属性（Property）

var propertyType = propertyInfo.PropertyType;

if (propertyType == typeof(double))

{

propertyInfo.SetValue(target, System.Convert.ToDouble(value), new object\[0\]);

}

else if (propertyType == typeof(int) || propertyType.IsEnum)

{

propertyInfo.SetValue(target, System.Convert.ToInt32(value), new object\[0\]);

}

else if (propertyType == typeof(bool))

{

propertyInfo.SetValue(target, System.Convert.ToBoolean(value), new object\[0\]);

}

else if (propertyType == typeof(string))

{

propertyInfo.SetValue(target, System.Convert.ToString(value), new object\[0\]);

}

else if (value is NXOpen.Point3d)

{

propertyInfo.SetValue(target, (NXOpen.Point3d)(value), new object\[0\]);

}

else if (value is NXOpen.Vector3d)

{

propertyInfo.SetValue(target, (NXOpen.Vector3d)(value), new object\[0\]);

}

}

}

图1

}

else if (value is NXOpen.Point3d)

{

propertyInfo.SetValue(target, (NXOpen.Point3d)(value), new object\[0\]);

}

else if (value is NXOpen.Vector3d)

{

propertyInfo.SetValue(target, (NXOpen.Vector3d)(value), new object\[0\]);

}

}

}

2.用法如图2所示，即可快速自动读取表达式，并分别将表达式lh\_l、lh\_w、lh\_h的值依次赋值给Length、Width、Height三个属性。

public class TestClass

{

public TestClass(NXOpen.Assemblies.Component component)

{

var part = component?.Prototype as NXOpen.Part;

//调用方法，自动获取表达式值，修改属性成员值

this.PropertyFromExpression(part);

}

\[ExpressionAttribute("lh\_l")\]

public double Length { get; set; }

\[ExpressionAttribute("lh\_w")\]

public double Width { get; set; }

\[ExpressionAttribute("lh\_h")\]

public double Height { get; set; }

}

图2