# NX二次开发-巧用.NET的反射功能 - 知乎
作者：谭弘利 审校：凌俊

适用版本：NX 7.0及以上版本

一、概述

反射是.NET中的重要机制，通过反射，可以在运行时动态获得程序或程序集中每一个类型（包括类、结构、委托、接口和枚举等）的成员和成员的信息。有了反射，即可对每一个类型了如指掌。另外可以直接创建对象，即使这个对象的类型在编译时还不知道；也可以直接调用非公开的任何函数、属性和字段值。

二、详细说明

1\. 不抛出异常获取TaggedObject对象的Tag。在NXOpen开发中，有时需要转到调用UF函数，这时候需要获取对象的Tag值，但是有时候由于对象被删除或者撤销等操作，导致使用属性Tag时，偶尔会提示试图使用不活动的对象，UF里面有提供验证Tag状态的方法，但是NXOpen里面又没有对应的验证方法。如果使用UF的方法，还是需要先获取对象的Tag标识，此时使用属性Tag则会抛出异常。此时我们就可以使用映射获取TaggedObject对象的字段m\_tag的值。代码如下：

public static NXOpen.Tag GetTag(this NXOpen.TaggedObject taggedObjectI)

{

if (taggedObjectI != null)

{

var bindingFlag = System.Reflection.BindingFlags.NonPublic |

System.Reflection.BindingFlags.Instance;

var tagFieldInfo = typeof(NXOpen.TaggedObject).GetField("m\_tag", bindingFlag);

var tag = tagFieldInfo?.GetValue(taggedObjectI) as NXOpen.Tag?;

if (tag != null) return tag.Value;

}

return NXOpen.Tag.Null;

}

图1

2.动态将NX对象属性（Attribute）设置到.NET对象实例的属性（Property），通过使用特性标记标记类的属性（Property），在运行时自动读取NX对象的属性值（Attribute），赋值给.NET对象实例的（Property）。

public static void AttributeToProperty(NXOpen.NXObject nxObject, object target)

{

//验证输入参数是否为空

if (target == null)

{

throw new System.ArgumentNullException(nameof(target));

}

if (nxObject == null)

{

throw new System.ArgumentNullException(nameof(nxObject));

}

//遍历实例对象的Property属性

foreach (var propertyInfo in target.GetType().GetProperties().Where(p => p.CanWrite))

{

//查找特性标记

var expAttribute = propertyInfo

.GetCustomAttributes(true)

.OfType<NxAttributeAttribute>()

.FirstOrDefault();

if (expAttribute == null)

{

continue;

}

string title = expAttribute.Title;

if (string.IsNullOrEmpty(title))

{

title = propertyInfo.Name;

}

//将值赋值到实例对象的属性（Property）

var propertyType = propertyInfo.PropertyType;

if (propertyType == typeof(double))

{

var value = nxObject.GetRealUserAttribute(title);

if (value == null)

{

continue;

}

propertyInfo.SetValue(target, System.Convert.ToDouble(value), new object\[0\]);

}

else if (propertyType == typeof(int) || propertyType.IsEnum)

{

var value = nxObject.GetIntegerUserAttribute(title);

if (value == null)

{

continue;

}

propertyInfo.SetValue(target, System.Convert.ToInt32(value), new object\[0\]);

}

else if (propertyType == typeof(string))

{

var value = nxObject.GetStringUserAttribute(title);

if (value == null)

{

continue;

}

propertyInfo.SetValue(target, System.Convert.ToString(value), new object\[0\]);

}

}

}

图2