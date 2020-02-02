# 表的创建



##  iS3AreaHandle类

iS3平台设定每张表的实体类都继承 `.\iS3.MiniServer\iS3.Core\iS3Core.cs`文件中的 `iS3AreaHandle`类，换言之，在不改变这个类的内容的情况下，每张表都要有其中的 `object_id` `ID` `Name` `Description` `FullName` 这五列，数据类型也在类`iS3AreaHandle`中有注明，其余列名用户可根据自己的数据对应命名。当然用户也可以根据自己数据的特点对 `iS3AreaHandle`类进行修改，相应地，表的公共属性也进行对应的改变。

## 建表

Entity Framework（实体框架）是ADO.NET中的一组支持开发面向数据的软件应用程序的技术，是微软的一个ORM (O/R Mapping) 框架。ORM（对象关系映射框架）：指的是面向对象的对象模型和关系型数据库的数据结构之间的相互转换。

也就是说业务实体在内存中表现为对象，在数据库中表现为数据，内存中的对象之间，存在关联和继承关系，而在数据库中，关系数据无法直接表达这些关系。而对象--关系映射（ORM）就是解决这一问题的。ORM作为一个中间件，实现程序对象到关系数据库的数据映射。

那么，EF也就是一种实现数据库和程序中的实体相互映射的一种工具。

Entity Framework Code First与数据表之间有两种映射方式，可选择其中的某种方式来创建表。（详见[第一节 表名及所有者(三、附录/第一章 EFCF属性映射约定/第一节 表名及所有者 )](../../../chapter3/section1/appendix1.md)）

以 Data Annotation 方式，创建实体`borehole`的表为例：（`borehole.cs`代码附于文末）

1. 在Server端添加`borehole.cs`文件，这相当于创建上文的对象；

2. 在代码中设定Table名称为`Geology_Borehole`，那么映射到数据库的表名就是`Geology_Borehole`。也可不设定表名，那么默认的数据表名是类名的复数，例，boreholes；

3. 通过映射自动产生SQL语句，在默认约定的情况下，映射创建的列名与类的属性名相同，当然也可以根据需要进行重新指定类属性与列名之间的映射关系；（详见[第二节 字段名、长度、数据类型(三、附录/第一章 EFCF属性映射约定 )](../../../chapter3/section1/appendix2.md)）

    以表`Geology_Borehole`为例， 映射对应的DDL（数据库定义语言）如下：

 ```sql
           create table Geology_Borehole( 
               #以下是对borehole属性的定义，左边一列是属性，右边一列是属性的数据类型
               object_id       int           not null,
               ID              int,
               Name            varchar(20)   not null
                   constraint Geology_Borehole_pk
                       primary key nonclustered,
               FullName        nvarchar(max) not null,
               Description     nvarchar(max),
               #以上是 iS3AreaHandle 类中定义的属性，下面是borehole自身的属性
               StratumSection  int,
               SectionSequence int,
               BoreholeType    nvarchar(max) not null,
               TopElevation    float         not null,
               BoreholeLength  float         not null,
               Mileage         nvarchar(max),
               Xcoordinate     float,
               Ycoordinate     float,
               OBJECTID        nvarchar(max)
           )
           go
 ```

4. 在上一节（[配置数据库信息(二、平台使用/第一章 数据准备/第一节 数据库配置/2. 配置数据库信息)](./detail3.md)）配置的数据库对应的表（即本例中的`Geology_Borehole`）中导入配置文件`geodatabase`中的数据。

   到此，以建立`borehole`的表为例子，建表完成。

   

   本例中`borehole.cs`的代码如下：

   ```csharp
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
      //而NotMapped特性打破了这个约定，你可以使用NotMapped特性到某个属性上面，然后Code-First就不会为这个属性在数据表中创建列了。
   
          [NotMapped]
          public List<BoreholeStrata> BoreholeStratas { get; set; } = new List<BoreholeStrata>();
      }
      }
   
   
   ```

   



