# 数据配置

平台根据数据用途将数据分为多个`Domain`类，在这里我们以地质数据`Geology`和监测点数据`Monitoring`为例，介绍`Domain`类下数据对象`DGObject`模型的配置方法。

### Geology

Geology的数据放在`iS3.MiniServer/ iS3.Geology/ Model`中，其中记录着表的列名（不包括类`iS3AreaHandle`中的五个列名）和数据类型；
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
    [Table("Geology_Borehole")]
    public partial class Borehole:iS3AreaHandle
    {
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
        [NotMapped]
        public List<BoreholeStrata> BoreholeStratas { get; set; } = new List<BoreholeStrata>();
    }
}

```

### Monitoring

Monitoring的数据放在`iS3.MiniServer/ iS3.Monitoring/ Model`中，其中记录着表的列名（不包括类`iS3AreaHandle`中的五个列名）和数据类型。
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

        [NotMapped]
        // Summary:
        //    readings dictionary - reading component indexed 
        public Dictionary<string, List<MonReading>> readingsDict;
        [NotMapped]
        public List<MonComponent> monComponentList;
    }
}
```

