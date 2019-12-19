# 扩展类的实现

本小节将对`Extensions`类进行说明，并引导用户定义一个 `EntryPoint`类。 `EntryPoint`类是扩展库的入口，继承自`iS3.Core`中的`Extensions`类。通过`EntryPoint`类，主程序运行时才能识别用户自定义的扩展类。

### 1. Extensions类说明

`iS3.Core`中的`Extensions`类是用户实现自定义扩展类所依赖的重要对象。所有用户自定义的扩展类均需继承自该`Extensions`类，否则无法实现扩展。`Extensions`类的定义如下所示：

```c#
	public class Extensions
    {
        // Summary:
        //     Name, version and provide of the extension
        public virtual string name() { return "Unknown extension"; }
        public virtual string version() { return "Unknown"; }
        public virtual string provider() { return "Unknown provider"; }

        // Summary:
        //     Initialize the extension, called immediately after loaded.
        // Return value:
        //     A string that will be printed in output window.
        public virtual string init() 
        {
            string msg = String.Format("Loaded {0} by {1}, version {2}.\n",
                name(), provider(), version());
            return msg; 
        }
    }
```

自定义的扩展类继承自`Extensions`类后，编译生成的动态链接文件应该放置在`\bin\extensions`目录下，否则无法被主程序识别。`iS3.Core`中的`ExtensionsManager`类将读取放置于`\bin\extensions`目录下的dll文件，从类名中创建出实体。



### 2. 实现EntryPoint类

此处，我们将演示如何实现扩展类的入口——`EntryPoint`类，并对其函数进行简要介绍。

以同济大学项目的演示为例，代码如下：

```cs
namespace iS3.Geology
{
    // Summary:
    //     This is the entry point for the extension
    public class EntryPoint : Extensions
    {
        public override string name() { return "iS3.Geology"; }
        public override string provider() { return "Tongji iS3 team"; }
        public override string version() { return "1.0"; }
    }
}
```

此处，我们首先需要继承`Extensions`类，以声明自定义类的扩展属性。其次，我们还需重载`Extensions`类的三个函数，尤其是 `name()` 函数，这是识别出自定义类所处的命名空间的关键。

**重载函数说明**

- `name()`：返回所处的命名空间。此处所处的`namespace`为`iS3.Geology`，所以应该返回"iS3.Geology"。
- `provider()` ：返回开发团队名称。
- `version()`：返回版本信息。



### 3. 实现Demo扩展工具

此处，我们将以`DemoTools`工具为例，介绍扩展工具的实现。

iS3 系统在 `iS3.Core` 中定义了 `Tools` 类，该类继承自 `Extensions` 类，用于实现用户自定义工具的扩展。用户自定义的工具均需继承自该类。`Tools` 类的定义如下所示：

```c#
    public class Tools : Extensions
    {
        // Summary:
        //     Name, version and provide of the tool
        public override string name() { return "Unknown tool"; }

        // Summary:
        //     Get treeItems of the tool, called immediately after loaded.
        public virtual IEnumerable<ToolTreeItem> treeItems()
        { 
            return null; 
        }
    }
```

首先，我们定义一个`DemoTool`类，该类继承自 `Tools` 类，实现的功能是点击工具面板的“DemoTool”字样时，自动在当前界面打开一个Demo图层。`DemoTool` 类的定义如下所示：

```c#
public class DemoTools : Tools
    {
        //基本信息
        public override string name() { return "iS3.DemoTools"; }
        public override string provider() { return "Tongji iS3 team"; }
        public override string version() { return "1.0"; }
        //分析工具列表
        List<ToolTreeItem> items;
        public override IEnumerable<ToolTreeItem> treeItems(){
            return items;
        }
        //新建分析工具窗口
        DemoWindow demoWindow;
        public void callDemoWindow(){
            if (demoWindow != null){
                demoWindow.Show();
                return;
            }

            demoWindow = new DemoWindow();
            demoWindow.Closed += (o, args) =>{
                    demoWindow = null;
                };
            demoWindow.Show();
        }
        //新建工具树
        public DemoTools(){
            items = new List<ToolTreeItem>();

            ToolTreeItem item = new ToolTreeItem("Demo|Basic", "DemoTest", callDemoWindow);
            items.Add(item);
        }
    }
```

然后，我们在`DemoWindow.xaml.cs`中定义DemoTool有关的视图和触发事件，详细可查看。由于代码较长，此处不复展开详述。此处，放出最终效果：

<div style= text-align:center>
<img src=".\img\demo.png" style='width:600px'; 'left: 50%'/>
</div>


#### 附件：DemoWindow.xaml.cs

```c#
public partial class DemoWindow : Window
{
    //定义全局变量
    Project _prj;
    Domain _structureDomain;
    IMainFrame _mainFrame;
    IView _inputView;

    //DGObject members
    DGObjectsCollection _allSLs; //所有衬砌对象
    List<string> _slLayerIDs; //衬砌图层ID
    Dictionary<string, IEnumerable<DGObject>> _selectedSLsDict; //选中的衬砌

    //graphics members
    ISpatialReference _spatialRef; //视图坐标系

    //result
    Dictionary<int, int> _slsGrade; //用来存储衬砌评估等级
    Dictionary<int, IGraphicCollection> _slsGraphics; //图形结果存储
    
    public DemoWindow()
    {
        InitializeComponent();

        //初始化全局变量
        _selectedSLsDict = new Dictionary<string, IEnumerable<DGObject>>();
        _slsGrade = new Dictionary<int, int>();
        _slsGraphics = new Dictionary<int, IGraphicCollection>();

        _mainFrame = Globals.mainframe;
        _prj = Globals.project;
        _structureDomain = _prj.getDomain(DomainType.Structure);
        _allSLs = _structureDomain.getObjects("SegmentLining");
        _slLayerIDs = new List<string>();
        foreach (DGObjects objs in _allSLs)
            _slLayerIDs.Add(objs.definition.GISLayerName);

        //窗口加载、关闭事件
        Loaded += DemoWindow_Loaded;
        Unloaded += DemoWindow_Unloaded;
    }

    //窗口加载事件
    void DemoWindow_Loaded(object sender,
        RoutedEventArgs e)
    {
        //设置窗口在右下角弹出
        Application curApp = Application.Current;
        Window mainWindow = curApp.MainWindow;
        this.Owner = mainWindow;
        this.Left = mainWindow.Left +
            (mainWindow.Width - this.ActualWidth - 10);
        this.Top = mainWindow.Top +
            (mainWindow.Height - this.ActualHeight - 10);

        //设置input view combobox数据源
        List<IView> planViews = new List<IView>();
        foreach (IView view in _mainFrame.views)
        {
            if (view.eMap.MapType == EngineeringMapType.FootPrintMap)
                planViews.Add(view);
        }
        InputCB.ItemsSource = planViews;
        if (planViews.Count > 0)
        {
            _inputView = planViews[0];
            InputCB.SelectedIndex = 0;
        }
        else
        {
            return;
        }

        //设置segmenglining listbox数据源
        _inputView_objSelectionChangedListener(null, null);
    }

    //窗口关闭事件
    void DemoWindow_Unloaded(object sender,
        RoutedEventArgs e)
    {
        _inputView.addSeletableLayer("_ALL");
        _inputView.objSelectionChangedTrigger -=
            _inputView_objSelectionChangedListener;
    }

    //视图对象选择监听事件
    void _inputView_objSelectionChangedListener(object sender,
        ObjSelectionChangedEventArgs e)
    {
        //设置segmenglining listbox数据源
        _selectedSLsDict = _prj.getSelectedObjs(_structureDomain, "SegmentLining");
        List<DGObject> _sls = new List<DGObject>();
        foreach (var item in _selectedSLsDict.Values)
        {
            foreach (var obj in item)
            {
                _sls.Add(obj);
            }
        }
        if (_sls != null && _sls.Count() > 0)
            SLLB.ItemsSource = _sls;
    }

    //input view cobombox选择事件
    private void InputCB_SelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        //上一次选择的view
        _inputView.addSeletableLayer("_ALL");
        _inputView.objSelectionChangedTrigger -=
                _inputView_objSelectionChangedListener;
        
        //新选择的view
        _inputView = InputCB.SelectedItem as IView;
        _inputView.removeSelectableLayer("_ALL");
        foreach (string layerID in _slLayerIDs)
            _inputView.addSeletableLayer(layerID);

        //为新的view添加对象选择监听事件
        _inputView.objSelectionChangedTrigger +=
            _inputView_objSelectionChangedListener;
    }

    //开始按钮事件
    private void Start_Click(object sender, RoutedEventArgs e)
    {
        StartAnalysis();
        SyncToView();
        Close();
    }

    //关闭按钮事件
    private void Cancel_Click(object sender, RoutedEventArgs e)
    {
        Close();
    }

    void StartAnalysis()
    {
        //获取输入的view和复制坐标系
        IView view = InputCB.SelectedItem as IView;
        _spatialRef = view.spatialReference;

        //开始分析
        foreach (string SLLayerID in _selectedSLsDict.Keys)
        {
            //获取衬砌选中列表
            IEnumerable<DGObject> sls = _selectedSLsDict[SLLayerID];
            List<DGObject> slList = sls.ToList();
            IGraphicsLayer gLayer = _inputView.getLayer(SLLayerID);             
            foreach(DGObject dg in slList)
            {
                //获取单个衬砌对象，计算评估等级
                SegmentLining sl = dg as SegmentLining;
                SLConvergenceRecordType slConvergenceRecordType = sl.ConstructionRecord.SLConvergenceRecords;
                if (slConvergenceRecordType.SLConvergenceItems.Count == 0)
                    continue;

                SLConvergenceItem slConvergenceItem = slConvergenceRecordType.SLConvergenceItems[0];
                if(slConvergenceItem.HorizontalDev == double.NaN)
                    continue;
                double raduis = (double)slConvergenceItem.HorizontalRad;
                double deviation = (double)slConvergenceItem.HorizontalDev;
                double ratio = deviation / (raduis - deviation) * 1000;

                int grade;
                if (ratio <= 3)
                    grade = 5;
                else if (ratio <= 5)
                    grade = 4;
                else if (ratio <= 8)
                    grade = 3;
                else if (ratio <= 10)
                    grade = 2;
                else
                    grade = 1;

                //根据评估等级获取图形样式
                ISymbol symbol = GetSymbol(grade);

                //为了演示，采用了较复杂的方法
                //<简便方法 可替换下面代码>
                //IGraphicCollection gcollection = gLayer.getGraphics(sl);
                //IGraphic g = gcollection[0];
                //g.Symbol = symbol;
                //IGraphicCollection gc = Runtime.graphicEngine.newGraphicCollection();
                //gc.Add(g);
                //_slsGraphics[sl.id] = gc;
                //</简便方法>

                //获取衬砌图形
                IGraphicCollection gcollection = gLayer.getGraphics(sl);
                IGraphic g = gcollection[0];
                IPolygon polygon = g.Geometry as IPolygon;
                IPointCollection pointCollection = polygon.GetPoints(); //获取端点
                //衬砌为长方形，故有四个点
                IMapPoint p1_temp = pointCollection[0];
                IMapPoint p2_temp = pointCollection[1];
                IMapPoint p3_temp = pointCollection[2];
                IMapPoint p4_temp = pointCollection[3];
                //新建新的点，注意复制坐标系
                IMapPoint p1 = Runtime.geometryEngine.newMapPoint(p1_temp.X, p1_temp.Y, _spatialRef);
                IMapPoint p2 = Runtime.geometryEngine.newMapPoint(p2_temp.X, p2_temp.Y, _spatialRef);
                IMapPoint p3 = Runtime.geometryEngine.newMapPoint(p3_temp.X, p3_temp.Y, _spatialRef);
                IMapPoint p4 = Runtime.geometryEngine.newMapPoint(p4_temp.X, p4_temp.Y, _spatialRef);
                //生成新的图形
                g = Runtime.graphicEngine.newQuadrilateral(p1, p2, p3, p4);
                g.Symbol = symbol;
                IGraphicCollection gc = Runtime.graphicEngine.newGraphicCollection();
                gc.Add(g);
                _slsGraphics[sl.id] = gc; //保存结果
            }
        }
    }
    
    //在view中加载图形，和同步图形
    void SyncToView()
    {
        IView view = InputCB.SelectedItem as IView;

        //为图形赋值“Name”属性，以便图形和数据关联
        foreach (int slID in _slsGraphics.Keys)
        {
            SegmentLining sl = _allSLs[slID] as SegmentLining;
            IGraphicCollection gc = _slsGraphics[slID];
            foreach (IGraphic g in gc)
                g.Attributes["Name"] = sl.name;
        }

        //将图形添加到view中
        string layerID = "DemoLayer"; //图层ID
        IGraphicsLayer gLayer = getDemoLayer(view, layerID); //获取图层函数
        foreach (int id in _slsGraphics.Keys)
        {
            IGraphicCollection gc = _slsGraphics[id];            
            gLayer.addGraphics(gc);
        }

        //使数据与图形关联
        List<DGObject> sls = _allSLs.merge();
        gLayer.syncObjects(sls);

        //计算新建图形范围，并在地图中显示该范围
        IEnvelope ext = null;
        foreach (IGraphicCollection gc in _slsGraphics.Values)
        {
            IEnvelope itemExt = GraphicsUtil.GetGraphicsEnvelope(gc);
            if (ext == null)
                ext = itemExt;
            else
                ext = ext.Union(itemExt);
        }
        _mainFrame.activeView = view;
        view.zoomTo(ext);
    }

    //根据评估等级获取样式
    ISymbol GetSymbol(int grade)
    {
        ISimpleLineSymbol linesymbol = Runtime.graphicEngine.newSimpleLineSymbol(
                            Colors.Black, SimpleLineStyle.Solid, 1.0);
        Color color = Colors.Green;
        if (grade == 5)
            color = Colors.LightGreen;
        if (grade == 4)
            color = Colors.LightYellow;
        if (grade == 3)
            color = Colors.LightSkyBlue;
        if (grade == 2)
            color = Colors.LightSalmon;
        if (grade == 1)
            color = Colors.LightPink;
        return Runtime.graphicEngine.newSimpleFillSymbol(color, SimpleFillStyle.Solid, linesymbol);
    }

    //获取新建图层
    IGraphicsLayer getDemoLayer(IView view, string layerID)
    {
        IGraphicsLayer gLayer = view.getLayer(layerID);
        if (gLayer == null)
        {
            gLayer = Runtime.graphicEngine.newGraphicsLayer(
                layerID, layerID);
            var sym_fill = GraphicsUtil.GetDefaultFillSymbol();
            var renderer = Runtime.graphicEngine.newSimpleRenderer(sym_fill);
            gLayer.setRenderer(renderer);
            gLayer.Opacity = 0.9;
            view.addLayer(gLayer);
        }
        return gLayer;
    }
}
```
