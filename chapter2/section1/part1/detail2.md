# 配置文件生成

## 步骤概览

- **Preparation Step 1**：配置iS3及数据目录的路径

- **Preparation Step 2**：配置项目地点和表述信息
- **Step 1 **：基本信息配置
- **Step 2**：2D模型配置（地图引擎配置）
- **Step 3**：配置项目的Domains信息
- **Step 4 **：配置项目树
-  **配置完成**

###  Preparation Step 1 -- 配置iS3及数据目录的路径

配置iS3的运行路径（即`iS3.Config.exe`所在文件夹路径）和数据目录的路径（即[数据文件准备（二、平台使用/第一章 数据准备/第二节 本地数据配置/1.数据文件准备）](./detail1.md)中存储项目数据文件和TPKS文件的文件夹路径）。默认情况下不用更改。

<div style= text-align:center>
<img src= "https://i.loli.net/2019/12/07/MF8cw1Y9QAmLp5O.png"   style='width:600px'; 'left: 50%'/>
</div>


点击**Start configuration**按钮开始配置。

###  Preparation Step 2 -- 配置项目地点和表述信息

配置该项目的名称，项目地点和表述信息。

点击加号按钮，添加项目，填写项目ID和描述信息。

<div style= text-align:center>
<img src= "https://i.loli.net/2019/12/07/rQRKMflY4bUF8jp.png" style='width:600px'; 'left: 50%'/>
</div>

选中新建的项目项，点击**Location**，在地图上选择项目的地点并点击。

<div style= text-align:center>
<img src= "https://i.loli.net/2019/12/07/2iUud5tgvok9GTC.png"  style='width:600px'; 'left: 50%'/>
</div>

如果想要修改描述信息，选中要修改的项目项，点击**Description**，可以对描述信息进行修改。

<div style= text-align:center>
<img src= "https://i.loli.net/2019/12/07/79cL5Wx3AQvefBs.png"  style='width:600px'; 'left: 50%'/>
</div>

配置好后点击**Next**按钮。

### Step 1 -- 基本信息配置

ID为项目ID，Title为项目名，`Local data path`为[数据文件准备（二、平台使用/第一章 数据准备/第二节 本地数据配置/1.数据文件准备）](./detail1.md)中存储项目数据文件，`Local tile path`为**数据文件准备**中TPKS文件的文件夹路径。这二者默认情况下无需更改。`Database`为数据库地址，分别替换掉IP地址、数据库名称、用户名称和密码。

<div style= text-align:center>
<img src="./img/step1.png"  style='width:600px'; 'left: 50%' />
</div>

配置好后点击**Next**按钮。

### Step 2 -- 2D模型配置（地图引擎配置）

点击左侧栏下方的添加按钮创建新的地图。

<div style= text-align:center>
<img src= "https://i.loli.net/2019/12/07/pdobq4Oyu3zx7XW.png" style='width:600px'; 'left: 50%'>
</div>

在右边栏填写地图ID（默认ID为`Map0`），选择地图类型（默认类型为`FootPrintMap`)。

以`TONGJI`项目为例，地图ID为`Map0`，地图类型为`FootPrintMap`。

<div style= text-align:center>
<img src= "https://i.loli.net/2019/12/07/aBAntRfpUjNwdXD.png" style='width:600px'; 'left: 50%'/>
</div>

点击右侧栏`Local tiled map`的按钮，选择该项目要使用的`tpk`文件。

<div style= text-align:center>
<img src= "https://i.loli.net/2019/12/07/KDv4Ekpmlc81JIG.png"  style='width:600px'; 'left: 50%'/>
</div>

点击右侧栏`Local GeoDB file`的按钮，选择该项目要用的`geodatabase`文件，并可以在`Geo Layers`里勾选要用的图层。

<div style= text-align:center>
<img src= "https://i.loli.net/2019/12/07/Izla5CBs3roAu8m.png"  style='width:600px'; 'left: 50%'/>
</div>

配置好后点击**Next**按钮。

### Step 3 -- 配置项目的Domains信息

点击左上角`Domains`栏下的添加按钮，在弹出的对话框里选择要选择的Domain类型。

<div style= text-align:center>
<img src= "https://i.loli.net/2019/12/07/7iTzRlEbdmSUDCM.png"   style='width:600px'; 'left: 50%'/>
</div>

如下图，选择`Geology`类。

<div style= text-align:center>
<img src= "https://ftp.bmp.ovh/imgs/2019/12/2133df19581a9e49.png"   style='width:600px'; 'left: 50%'/>
</div>

选中要添加的`Objects`所属的`Domain`，点击左下角`Digital objects`栏下的添加按钮，在弹出的对话框里填写该`Objects`的名字。

<div style= text-align:center>
<img src= "https://ftp.bmp.ovh/imgs/2019/12/ec70074e6b706097.png"  style='width:600px'; 'left: 50%'/>
</div>

选中要进行配置的`Domain`和`Digital object`，在右侧选择该`Digital object`的类型。

<div style= text-align:center>
<img src= "https://ftp.bmp.ovh/imgs/2019/12/adc6698e9bf9a626.png"   style='width:600px'; 'left: 50%'/>
</div>

填写`Table name`。点击`Table name`右侧的按钮，打开对话框，选择数据对应的表名。如图所示，此例选择`Geology_Borehole`。


<div style= text-align:center>
<img src="./img/step3-1.png"    style='width:600px'; 'left: 50%'/>
</div>

还可以对该表进行`sql`语句查询，即在`Condition(SQL)`项输入查询条件，在`Order(SQL)`项输入查询的数据以哪个属性排序。如图所示，查询`OBJECTID`大于3的数据，按`Name`排序。

<div style= text-align:center>
<img src="./img/step3-2.png"    style='width:600px'; 'left: 50%'/>
</div>

点击**Preview Table**按钮。

<div style= text-align:center>
<img src="./img/step3-3.png"   style='width:600px'; 'left: 50%'/>
</div>


如果该`Digital object`有对应的2D模型，则勾选**Has 2D Model**项，并点击其右侧的文件选择按钮，选择其对应的模型图层。

如果没有对应模型，则可以不勾选。

如下图所示，`Allboreholes`对应的2D模型为`Map0`的`Borehole`层。完成后点击OK。

<div style= text-align:center>
<img src= "https://ftp.bmp.ovh/imgs/2019/12/c85eb5b308f0b95d.png"  style='width:600px'; 'left: 50%'/>
</div>

完成后可以点击下方按钮**Preview 2D Layer...**进行查看。

<div style= text-align:center>
<img src= "https://ftp.bmp.ovh/imgs/2019/12/764f897c3aebab00.png"  style='width:600px'; 'left: 50%'/>
</div>

如果该`Digital object`有对应的3D模型，则勾选**Has 3D Model**项，并点击其右侧的文件选择按钮，选择其对应的模型图层。

如果没有对应模型，则可以不勾选。

如下图所示，`Allboreholes`对应的2D模型为`Borehole`层。完成后点击**OK**。

<div style= text-align:center>
<img src= "https://ftp.bmp.ovh/imgs/2019/12/60485233f5228648.png"  style='width:600px'; 'left: 50%'/>
</div>

完成后可以点击下方按钮**Preview 3D Model...**进行查看。

<div style= text-align:center>
<img src= "https://ftp.bmp.ovh/imgs/2019/12/d80412e28ccfbd7f.png"  style='width:600px'; 'left: 50%'/>
</div>

配置好后点击**Next**按钮。

### Step 4 -- 配置项目树

在左侧栏下方选中`Domain`，在左侧栏空白处点击鼠标右键，选择**Add**添加树。

<div style= text-align:center>
<img src= "https://ftp.bmp.ovh/imgs/2019/12/eb438e2a975b2941.png"   style='width:600px'; 'left: 50%'/>
</div>

填写根的`Name`和`Display Name`。

<div style= text-align:center>
<img src= "https://ftp.bmp.ovh/imgs/2019/12/55572021c84841fa.png"  style='width:600px'; 'left: 50%'/>
</div>

选中根项点击右键添加`Digital object`，并填写`Display Name`，选择`Name`和`Digital object`。

注意：在切换`Domain`的Tab前，务必先选中切换前`Domain`的根项，否则选中项的`Digital object`属性会丢失。

<div style= text-align:center>
<img src= "https://ftp.bmp.ovh/imgs/2019/12/f0044f80af2358ac.png"   style='width:600px'; 'left: 50%'/>
</div>

配置好后点击**Finish**按钮。

### 配置完成

配置成功后会弹出对话框如下图。

<div style= text-align:center>
<img src= "https://i.postimg.cc/vBTWBFzT/success.png"   style='width:600px'; 'left: 50%'/>
</div>

在`.\iS3-Desktop-Client\Output\Data`路径下也会生成一个项目配置文件。在该例中，出现`TONGJI.xml`文件。内容如下：

```xml
<Project>
<ProjectDefinition ID="TONGJI" ProjectTitle="TONGJI" DefaultMapID="{assembly:Null}" LocalFilePath="Data\TONGJI" LocalTilePath="Data\TPKs" LocalDatabaseName="C:\Users\DELL\Desktop\IS3-2.0\iS3_2.0_Demo-master\iS3_2.0_Demo-master\iS3-Desktop-Client\Output\Data\TONGJI\TONGJI.MDB" DataServiceUrl="{assembly:Null}" GeometryServiceUrl="{assembly:Null}" xmlns="clr-namespace:iS3.Core;assembly=iS3.Core" xmlns:assembly="http://schemas.microsoft.com/winfx/2006/xaml">
  <ProjectDefinition.EngineeringMaps>
<EngineeringMap MapType="FootPrintMap" Calibrated="False" Scale="1" ScaleX="1" ScaleY="1" ScaleZ="1" MapID="Map0" LocalTileFileName1="tongji.tpk" LocalTileFileName2="{assembly:Null}" LocalMapFileName="{assembly:Null}" LocalGeoDbFileName="tongji.geodatabase" MapUrl="{assembly:Null}" XMax="13527374.886486448" XMin="13524718.993674662" YMax="3670398.5471424954" YMin="3668114.5746885533" MinimumResolution="0" MapRotation="0" xmlns="clr-namespace:iS3.Core;assembly=iS3.Core" xmlns:assembly="http://schemas.microsoft.com/winfx/2006/xaml">
  <EngineeringMap.LocalGdbLayersDef><LayerDef Name="Borehole" GeometryType="Point" IsVisible="True" Color="#FFFF0000" SelectionColor="#FFFF0000" MarkerSize="12" MarkerStyle="Circle" LineStyle="Solid" FillStyle="Solid" OutlineColor="#FF000000" LineWidth="1" RendererDef="{assembly:Null}" EnableLabel="True" LabelTextExpression="[Name]" LabelWhereClause="{assembly:Null}" LabelColor="#FF000000" LabelBackgroundColor="#00000000" LabelBorderLineColor="#00000000" LabelBorderLineWidth="0" LabelFontFamily="Arial" LabelFontSize="12" LabelFontStyle="Normal" LabelFontWeight="Normal" LabelTextDecoration="None" xmlns="clr-namespace:iS3.Core;assembly=iS3.Core" xmlns:assembly="http://schemas.microsoft.com/winfx/2006/xaml" />
<LayerDef Name="B1" GeometryType="Polyline" IsVisible="True" Color="#FF008000" SelectionColor="#FFFF0000" MarkerSize="12" MarkerStyle="Circle" LineStyle="Solid" FillStyle="Solid" OutlineColor="#FF008000" LineWidth="1" RendererDef="{assembly:Null}" EnableLabel="False" LabelTextExpression="{assembly:Null}" LabelWhereClause="{assembly:Null}" LabelColor="#FF000000" LabelBackgroundColor="#00000000" LabelBorderLineColor="#00000000" LabelBorderLineWidth="0" LabelFontFamily="Arial" LabelFontSize="12" LabelFontStyle="Normal" LabelFontWeight="Normal" LabelTextDecoration="None" xmlns="clr-namespace:iS3.Core;assembly=iS3.Core" xmlns:assembly="http://schemas.microsoft.com/winfx/2006/xaml" />
<LayerDef Name="Mon" GeometryType="Point" IsVisible="True" Color="#FFFF0000" SelectionColor="#FFFF0000" MarkerSize="12" MarkerStyle="Circle" LineStyle="Solid" FillStyle="Solid" OutlineColor="#FF000000" LineWidth="1" RendererDef="{assembly:Null}" EnableLabel="True" LabelTextExpression="[Name]" LabelWhereClause="{assembly:Null}" LabelColor="#FF000000" LabelBackgroundColor="#00000000" LabelBorderLineColor="#00000000" LabelBorderLineWidth="0" LabelFontFamily="Arial" LabelFontSize="12" LabelFontStyle="Normal" LabelFontWeight="Normal" LabelTextDecoration="None" xmlns="clr-namespace:iS3.Core;assembly=iS3.Core" xmlns:assembly="http://schemas.microsoft.com/winfx/2006/xaml" />
</EngineeringMap.LocalGdbLayersDef>
</EngineeringMap>
</ProjectDefinition.EngineeringMaps>
</ProjectDefinition>

  <Domain Name="Geology" Type="Geology">
    <ObjsDefinition>
      <Borehole Name="Allboreholes" HasGeometry="true" GISLayerName="Borehole" Has3D="true" Layer3DName="iS3Project/Borehole" />
      <Stratum Name="AllStratum" HasGeometry="false" Has3D="false" />
      <SoilProperty Name="AllSoilProperties" HasGeometry="false" Has3D="false" />
    </ObjsDefinition>
    <TreeDefinition>
      <Geology>
        <EngineeringGeology DisplayName="Engineering Geology" RefDomainName="Geology">
          <Borehole DisplayName="Boreholes" RefDomainName="Geology" RefObjsName="Allboreholes" />
          <Stratum DisplayName="Stratum" RefDomainName="Geology" RefObjsName="AllStratum" />
          <SoilProperty DisplayName="Soil Properties" RefDomainName="Geology" RefObjsName="AllSoilProperties" />
        </EngineeringGeology>
      </Geology>
    </TreeDefinition>
  </Domain>
  <Domain Name="Monitoring" Type="Monitoring">
    <ObjsDefinition>
      <MonPoint Name="monpoint" HasGeometry="true" GISLayerName="Mon" Has3D="true" Layer3DName="iS3Project/Mon" />
    </ObjsDefinition>
    <TreeDefinition>
      <Monitoring>
        <MonitoringItem DisplayName="监测" RefDomainName="Monitoring">
          <MonPoint DisplayName="监测点" RefDomainName="Monitoring" RefObjsName="monpoint" />
        </MonitoringItem>
      </Monitoring>
    </TreeDefinition>
  </Domain>
</Project>
```

项目配置文件如若理解透彻，也可以自己手动编写。

