# 实现数字化对象类

这些类就是所需实现的扩展类，也是在项目中要被渲染出来的类，诸如钻孔信息，土质信息等。该类的实现需要继承自`iS3.Core`中的数字化对象基类，即`DGObject`。这些类需要有一些属性和基本的`ToString`方法和`idFilter`方法，此外，还可以有为渲染`chartViews`而进行准备的数据处理方法。

此处，我们以`Borehole.cs`为例进行介绍。`Borehole.cs`源代码如下

```csharp
namespace iS3.Geology
{
   
    //定义Borehole地质对象，用于图表视窗的图形渲染
    public class BoreholeGeology
    {
        public double Top { get; set; }
        public double Base { get; set; }
        public int StratumID { get; set; }
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

