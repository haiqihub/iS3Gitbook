# 实现数字化对象实体类

数字化对象实体类就是所需实现的扩展类，也是在项目中要被渲染出来的类，诸如钻孔信息，土质信息等。该类的实现需要继承自`iS3.Core`中的数字化对象基类，即`DGObject`。这些类需要有一些属性和基本的`ToString`方法和`idFilter`方法，此外，还可以有为渲染`chartViews`而进行准备的数据处理方法。

此处，我们以`Borehole`钻孔类为例进行介绍。

### 1. 属性部分

在`DGObject`基类中，我们定义了部分通用的属性，如`_id`、`_name`、`_fullname`等。但这些属性无法满足个性化的需求，对于特定的数字化对象实体，我们需要在对应类中声明其拥有的属性，并通过`get`、`set`方式与数据库中的数据建立映射关系。这些属性将会在iS3平台中展示，或者在该类定义的函数中被调用。

`Borehole`类的属性如下所示：

| 序号 |    属性名称     |       属性类型        |    简要说明    |
| :--: | :-------------: | :-------------------: | :------------: |
|  1   |       Top       |        double         |  钻孔顶部高度  |
|  2   |      Base       |        double         |  钻孔底部高度  |
|  3   |     Mileage     |        double?        |   钻孔里程数   |
|  4   |      Type       |        string         |      类型      |
|  5   |    Geologies    | List<BoreholeGeology> | 钻孔内地层序列 |
|  6   | StratumSection  |     Nullable<int>     |    地层剖面    |
|  7   | SectionSequence |     Nullable<int>     |    剖面序列    |
|  8   |  BoreholeType   |        string         |    钻孔类型    |
|  9   |  TopElevation   |        double         |    最高海拔    |
|  10  | BoreholeLength  |        double         |    钻孔长度    |
|  11  |   Xcoordinate   |   Nullable<double>    |    X轴坐标     |
|  12  |   Ycoordinate   |   Nullable<double>    |    Y轴坐标     |

代码定义如下：
```csharp
        //自定义的属性值，通过get、set方式与数据库中的数据建立映射
        public double Top { get; set; }
        public double Base { get; set; }
        public double? Mileage { get; set; }
        public string Type { get; set; }
        public List<BoreholeGeology> Geologies { get; set; }
        public Nullable<int> StratumSection { get; set; }
        public Nullable<int> SectionSequence { get; set; }
        public string BoreholeType { get; set; }
        public double TopElevation { get; set; }
        public double BoreholeLength { get; set; }
        public Nullable<double> Xcoordinate { get; set; }
        public Nullable<double> Ycoordinate { get; set; }
```

最终结果中将会在Data List板块展示`Borehole`类定义的上述属性数据（具体数据由数据库提供），如下图所示：

<img src="../img/client8.png" style="zoom:67%;" />



### 2. 函数部分

自定义数字化对象实体类时，需对`DGOject`基类的方法进行重载，在此也可以自定义有关函数（此处不做演示）。

| **序号** | **方法名称** |                      **传入参数**                       |      **返回类型**      |                   **简要说明**                   |
| :------: | :----------: | :-----------------------------------------------------: | :--------------------: | :----------------------------------------------: |
|    1     |   Borehole   |                          void                           |        Borehole        |                 初始化Borehole类                 |
|    2     |   ToString   |                          void                           |         string         | 重载Tostring类，即定义Borehole类的字符串输出格式 |
|    3     |   idFilter   |               IEnumerable<DGObject> objs                |         string         |     返回Borehole类型的数据集合中所有对象的ID     |
|    4     |  chartViews  | IEnumerable<DGObject> objs, double width, double height | List<FrameworkElement> |              重载chartView图表视窗               |

代码实现如下：
```csharp
		//初始化Borehole类
        public Borehole()
        {
            Geologies = new List<BoreholeGeology>();
        }

        //重载Tostring类，即定义Borehole类的字符串输出格式
        public override string ToString()
        {
            string str = base.ToString();

           string str1 = string.Format(
                ", Top={0}, Base={1}, Mileage={2}, Type={3}, Geo=",
                Top, Base, Mileage, Type);
            str += str1;

            foreach (var geo in Geologies)
            {
                str += geo.StratumID + ",";
            }

            return str;
        }

        //返回Borehole类型的数据集合中所有对象的ID
        string idFilter(IEnumerable<DGObject> objs)
        {
            string sql = "BoreholeID in (";
            foreach (var obj in objs)
            {
                sql += obj.ID.ToString();
                sql += ",";
            }
            sql += ")";
            return sql;
        }

        //重载chartView图表视窗
        public override List<FrameworkElement> chartViews(
            IEnumerable<DGObject> objs, double width, double height)
        {
            //新建图表对象列表
            List<FrameworkElement> charts = new List<FrameworkElement>();

            //新建Borehole数组，将objs集合中的DGObject对象放入
            List<Borehole> bhs = new List<Borehole>();
            foreach (Borehole bh in objs)
            {
                if (bh != null && bh.Geologies.Count > 0)
                    bhs.Add(bh);
            }

            //定位Geology类型的Domain
            Domain geologyDomain = Globals.project.getDomain(DomainType.Geology);
            //查找Geology Domain下所有Stratum类型的数字化对象，并得到一个数组
            DGObjectsCollection strata = geologyDomain.getObjects("Stratum");

            //新建Borehole集合视图，并对其属性进行定义
            BoreholeCollectionView bhsView = new BoreholeCollectionView();
            bhsView.Name = "Geology";
            bhsView.Boreholes = bhs;
            bhsView.Strata = strata;
            bhsView.ViewerHeight = height;
            //刷新视图
            bhsView.RefreshView();
            bhsView.UpdateLayout();
            charts.Add(bhsView);

            return charts;
        }
```



### 3. 辅助类

#### 3.1 BoreholeGeology

**简要说明**

钻孔内地层，涉及Borehole图表视窗的图形渲染。

**实现代码**

```csharp
    public class BoreholeGeology
    {
        public double Top { get; set; }//顶部高度
        public double Base { get; set; }//底部高度
        public int StratumID { get; set; }//地层序号
    }
```



### 4. 源代码

`Borehole.cs`的完整源代码如下：

```csharp
namespace iS3.Geology
{
   
    //钻孔内地层，涉及Borehole图表视窗的图形渲染。
    public class BoreholeGeology
    {
        public double Top { get; set; }//顶部高度
        public double Base { get; set; }//底部高度
        public int StratumID { get; set; }//地层序号
    }

    // 自定义的数字化对象类，需继承自DGObject
    public class Borehole : DGObject
    {
        //自定义的属性值，通过get、set方式与数据库中的数据建立映射
        public double Top { get; set; }
        public double Base { get; set; }
        public double? Mileage { get; set; }
        public string Type { get; set; }
        public List<BoreholeGeology> Geologies { get; set; }
        public Nullable<int> StratumSection { get; set; }
        public Nullable<int> SectionSequence { get; set; }
        public string BoreholeType { get; set; }
        public double TopElevation { get; set; }
        public double BoreholeLength { get; set; }
        public Nullable<double> Xcoordinate { get; set; }
        public Nullable<double> Ycoordinate { get; set; }

		//初始化Borehole类
        public Borehole()
        {
            Geologies = new List<BoreholeGeology>();
        }

        //重载Tostring类，即定义Borehole类的字符串输出格式
        public override string ToString()
        {
            string str = base.ToString();

           string str1 = string.Format(
                ", Top={0}, Base={1}, Mileage={2}, Type={3}, Geo=",
                Top, Base, Mileage, Type);
            str += str1;

            foreach (var geo in Geologies)
            {
                str += geo.StratumID + ",";
            }

            return str;
        }

        //返回Borehole类型的数据集合中所有对象的ID
        string idFilter(IEnumerable<DGObject> objs)
        {
            string sql = "BoreholeID in (";
            foreach (var obj in objs)
            {
                sql += obj.ID.ToString();
                sql += ",";
            }
            sql += ")";
            return sql;
        }

        //重载chartView图表视窗
        public override List<FrameworkElement> chartViews(
            IEnumerable<DGObject> objs, double width, double height)
        {
            //新建图表对象列表
            List<FrameworkElement> charts = new List<FrameworkElement>();

            //新建Borehole数组，将objs集合中的DGObject对象放入
            List<Borehole> bhs = new List<Borehole>();
            foreach (Borehole bh in objs)
            {
                if (bh != null && bh.Geologies.Count > 0)
                    bhs.Add(bh);
            }

            //定位Geology类型的Domain
            Domain geologyDomain = Globals.project.getDomain(DomainType.Geology);
            //查找Geology Domain下所有Stratum类型的数字化对象，并得到一个数组
            DGObjectsCollection strata = geologyDomain.getObjects("Stratum");

            //新建Borehole集合视图，并对其属性进行定义
            BoreholeCollectionView bhsView = new BoreholeCollectionView();
            bhsView.Name = "Geology";
            bhsView.Boreholes = bhs;
            bhsView.Strata = strata;
            bhsView.ViewerHeight = height;
            //刷新视图
            bhsView.RefreshView();
            bhsView.UpdateLayout();
            charts.Add(bhsView);

            return charts;
        }
    }
}
```



### 5. 最终效果

完成后的最终效果图如下所示：

<img src="../img/client9.png" style="zoom:67%;" />