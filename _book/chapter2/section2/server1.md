# 数据配置



平台根据数据用途将数据分为多个`Domain`类，在这里我们以地质数据`Geology`和监测点数据`Monitoring`为例，介绍`Domain`类下数据对象`DGObject`模型的配置方法。

其中，`Geology`和`Monitoring`运用了`EntityFramework`映射中的`Data Annotation`方式（详见[EFCF属性映射约定(三、附录/第一章 EFCF属性映射规定)](./../../chapter3/appendix.md)）

### Geology

`Geology`的数据放在`.\iS3.MiniServer\iS3.Geology\Model`中，其中记录着表的属性和对应的数据类型。（不包括类`iS3AreaHandle`中的五个属性及其数据类型，关于类`iS3AreaHandle`详见[表的创建(二、平台使用/第一章 数据准备/第一节 数据库配置/2. 表的创建)](./../section1/part2/detail2.md)中的 `iS3AreaHandle`类）

以`borehole.cs`（钻孔）为例：

```cs
using System;
using System.Collections.Generic;
using System.ComponentModel.DataAnnotations.Schema;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using iS3.Core;
namespace iS3.Geology.Model
{
//指定表名为"Geology_Borehole"
    [Table("Geology_Borehole")]
    public partial class Borehole:iS3AreaHandle
    {     
//以下是表"Geology_Borehole"的属性及其对应的数据类型
//{ get; set; } get 是读取属性时进行的操作，set 是设置属性时进行的操作。
        public string OBJECTID { get; set; }
        public string Name { get; set; }
        public string FullName { get; set; }
        public string Description { get; set; }
        public Nullable<int> StratumSection { get; set; }
        public Nullable<int> SectionSequence { get; set; }
        public string BoreholeType { get; set; }
        public double TopElevation { get; set; }
        public double BoreholeLength { get; set; }
        public string Mileage { get; set; }
        public Nullable<double> Xcoordinate { get; set; }
        public Nullable<double> Ycoordinate { get; set; }

//NotMapped特性可以应用到领域类的属性中，EF Code-First默认的约定:为所有带有get,和set属性选择器的属性创建数据列。
//而NotManpped特性打破了这个约定，你可以使用NotMapped特性到某个属性上面，然后Code-First就不会为这个属性在数据表中创建列了。

        [NotMapped]
        public List<BoreholeStrata> BoreholeStratas { get; set; } = new List<BoreholeStrata>();
    }
}

```

### Monitoring

`Monitoring`的数据放在`.\iS3.MiniServer\iS3.Monitoring\Model`中，其中记录着表的列名（不包括类`iS3AreaHandle`中的五个列名）和数据类型。

以`monpoint.cs`（监测点）为例：

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using iS3.Core;
using System.ComponentModel.DataAnnotations.Schema;

namespace iS3.Monitoring
{
//未指定表名，所以表名为默认"MonPoint"复数形式：MonPoints"
//以下是表"Monpoints"的属性及其对应的数据类型
//{ get; set; } get 是读取属性时进行的操作，set 是设置属性时进行的操作。
    public class MonPoint: iS3AreaHandle
    {
        // Summary:
        //    reference point name
        public string refPointName { get; set; }
        // Summary:
        //    distance to the reference point
        public Nullable<decimal> distanceX { get; set; }
        public Nullable<decimal> distanceY { get; set; }
        public Nullable<decimal> distanceZ { get; set; }
        // Summary:
        //    Installation date and time
        public Nullable<DateTime> time { get; set; }
        // Summary:
        //    Instrument detail
        public string instrumentDetail { get; set; }
        // Summary:
        //    Bearing of monitoring axis (A, B, C): in degree
        public Nullable<decimal> bearingA { get; set; }
        public Nullable<decimal> bearingB { get; set; }
        public Nullable<decimal> bearingC { get; set; }
        // Summary:
        //    Inclination of instrument axis (A, B, C): in degree
        public Nullable<decimal> inclinationA { get; set; }
        public Nullable<decimal> inclinationB { get; set; }
        public Nullable<decimal> inclinationC { get; set; }
        // Summary:
        //    Reading sign convention in direction (A, B, C)
        public string readingSignA { get; set; }
        public string readingSignB { get; set; }
        public string readingSignC { get; set; }
        // Summary:
        //    componennt count
        public int componentCount { get; set; }
        // Summary:
        //    Component names
        public string componentNames { get; set; }
        // Summary:
        //    Remarks
        public string remarks { get; set; }
        // Summary:
        //    Contractor who installed monitoring instrument
        public string contractor { get; set; }
        // Summary:
        //    Associated file reference
        public string fileName { get; set; }
        
//NotMapped特性可以应用到领域类的属性中，EF Code-First默认的约定:为所有带有get,和set属性选择器的属性创建数据列。
//而NotManpped特性打破了这个约定，你可以使用NotMapped特性到某个属性上面，然后Code-First就不会为这个属性在数据表中创建列了。

        [NotMapped]
        // Summary:
        //    readings dictionary - reading component indexed 
        public Dictionary<string, List<MonReading>> readingsDict;
        [NotMapped]
        public List<MonComponent> monComponentList;
    }
}
```

