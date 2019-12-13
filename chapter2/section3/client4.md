# 实现数字对象类

这些类是`DGObject`的子类，也是在项目中要被渲染出来的类，诸如钻孔信息，土质信息等。这些类需要有一些属性和基本的`ToString`方法和`idFilter`方法，此外，还可以有为渲染`chartViews`而进行准备的数据处理方法。

以`borehole.cs`为例：

```cs
namespace iS3.Geology
{
   
    public class BoreholeGeology
    {
        public double Top { get; set; }
        public double Base { get; set; }
        public int StratumID { get; set; }
    }

    public class Borehole : DGObject
    {
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




        



        public Borehole()
        {
            Geologies = new List<BoreholeGeology>();
        }


       

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

        public override List<FrameworkElement> chartViews(
            IEnumerable<DGObject> objs, double width, double height)
        {
            List<FrameworkElement> charts = new List<FrameworkElement>();

            List<Borehole> bhs = new List<Borehole>();
            foreach (Borehole bh in objs)
            {
                if (bh != null && bh.Geologies.Count > 0)
                    bhs.Add(bh);
            }

            Domain geologyDomain = Globals.project.getDomain(DomainType.Geology);
            DGObjectsCollection strata = geologyDomain.getObjects("Stratum");

            BoreholeCollectionView bhsView = new BoreholeCollectionView();
            bhsView.Name = "Geology";
            bhsView.Boreholes = bhs;
            bhsView.Strata = strata;
            bhsView.ViewerHeight = height;
            bhsView.RefreshView();
            bhsView.UpdateLayout();
            charts.Add(bhsView);

            return charts;
        }
    }
}
```

