# 配置文件生成



###  Preparation Step 1 -- 配置iS3及数据目录的路径

​	配置iS3的运行路径（即`iS3.Config.exe`所在文件夹路径）和数据目录的路径（即[数据文件准备](./detail1.md)中存储项目数据文件和TPKS文件的文件夹路径）。默认情况下不用更改。

<img src= "https://i.loli.net/2019/12/07/MF8cw1Y9QAmLp5O.png" align=center height="300px" width="600px"/>



​	点击`Start configuration`按钮开始配置。

###  Preparation Step 2 -- 配置项目地点和表述信息

​	配置该项目的名称，项目地点和表述信息。

​	点击加号按钮，添加项目，填写项目ID和描述信息。

<img src= "https://i.loli.net/2019/12/07/rQRKMflY4bUF8jp.png" align=center height="300px" width="600px"/>

​	选中新建的项目项，点击Location，在地图上选择项目的地点并点击。

<img src= "https://i.loli.net/2019/12/07/2iUud5tgvok9GTC.png" align=center height="300px" width="600px"/>

​	如果想要修改描述信息，选中要修改的项目项，点击Description，可以对描述信息进行修改。

<img src= "https://i.loli.net/2019/12/07/79cL5Wx3AQvefBs.png" align=center height="300px" width="600px"/>

​	配置好后点击`Next`按钮。

### Step 1 -- 常规配置

​	ID为项目ID，Title为项目名，`Local data path`为[数据文件准备](./detail1.md)中存储项目数据文件，`Local tile path`为**数据文件准备**中TPKS文件的文件夹路径。默认情况下无需更改。

<img src= "https://i.loli.net/2019/12/07/YFCsimkxRfzHJKU.png" align=center height="450px" width="700px"/>

​	配置好后点击`Next`按钮。

### Step 2 -- 2D模型配置（地图引擎配置）

​	点击左侧栏下方的添加按钮创建新的地图。

<img src= "https://i.loli.net/2019/12/07/pdobq4Oyu3zx7XW.png" align=center height="300px" width="600px"/>

​	在右边栏填写地图ID（默认ID为`Map0`），选择地图类型（默认类型为`FootPrintMap`)。

​	以TONGJI项目为例，地图ID为`Map0`，地图类型为`FootPrintMap`。

<img src= "https://i.loli.net/2019/12/07/aBAntRfpUjNwdXD.png" align=center height="300px" width="600px"/>

​	点击右侧栏`Local tiled map`的按钮，选择该项目要使用的`tpk`文件。

<img src= "https://i.loli.net/2019/12/07/KDv4Ekpmlc81JIG.png" align=center height="300px" width="600px"/>

​	点击右侧栏`Local GeoDB file`的按钮，选择该项目要用的`geodatabase`文件，并可以在`Geo Layers`里勾选要用的图层。

<img src= "https://i.loli.net/2019/12/07/Izla5CBs3roAu8m.png" align=center height="300px" width="600px"/>

​	配置好后点击`Next`按钮。

### Step 3 -- 配置项目的Domains信息

​	点击左上角`Domains`栏下的添加按钮，在弹出的对话框里选择要选择的Domain类型。

<img src= "https://i.loli.net/2019/12/07/7iTzRlEbdmSUDCM.png" align=center height="300px" width="600px"/>

​	如下图，选择`Geology`类。

<img src= "https://ftp.bmp.ovh/imgs/2019/12/2133df19581a9e49.png" align=center height="300px" width="600px"/>

​	选中要添加的`Objects`所属的`Domain`，点击左下角`Digital objects`栏下的添加按钮，在弹出的对话框里填写该`Objects`的名字。

<img src= "https://ftp.bmp.ovh/imgs/2019/12/ec70074e6b706097.png" align=center height="300px" width="600px"/>

​	选中要进行配置的`Domain`和`Digital object`，在右侧选择该`Digital object`的类型。

<img src= "https://ftp.bmp.ovh/imgs/2019/12/adc6698e9bf9a626.png" align=center height="300px" width="600px"/>

​	Table name`，`Condition(SQL)`和`Order(SQL)无需填写。

​	如果该`Digital object`有对应的2D模型，则勾选`Has 2D Model`项，并点击其右侧的文件选择按钮，选择其对应的模型图层。

​	如果没有对应模型，则可以不勾选。

​	如下图所示，`Allboreholes`对应的2D模型为`Map0`的`Borehole`层。完成后点击OK。

<img src= "https://ftp.bmp.ovh/imgs/2019/12/c85eb5b308f0b95d.png" align=center height="300px" width="600px"/>

​	完成后可以点击下方按钮`Preview 2D Layer...`进行查看。

<img src= "https://ftp.bmp.ovh/imgs/2019/12/764f897c3aebab00.png" align=center height="300px" width="600px"/>

​	如果该`Digital object`有对应的3D模型，则勾选`Has 3D Model`项，并点击其右侧的文件选择按钮，选择其对应的模型图层。

​	如果没有对应模型，则可以不勾选。

​	如下图所示，`Allboreholes`对应的2D模型为`Borehole`层。完成后点击OK。

<img src= "https://ftp.bmp.ovh/imgs/2019/12/60485233f5228648.png" align=center height="300px" width="600px"/>

​	完成后可以点击下方按钮`Preview 3D Model...`进行查看。

<img src= "https://ftp.bmp.ovh/imgs/2019/12/d80412e28ccfbd7f.png" align=center height="300px" width="600px"/>

​	配置好后点击`Next`按钮。

### Step 4 -- 配置项目树

​	在左侧栏下方选中Domain，在左侧栏空白处点击鼠标右键，选择`Add`添加树。

<img src= "https://ftp.bmp.ovh/imgs/2019/12/eb438e2a975b2941.png" align=center height="300px" width="600px"/>

​	填写根的Name和Display Name。

<img src= "https://ftp.bmp.ovh/imgs/2019/12/55572021c84841fa.png" align=center height="300px" width="600px"/>

​	选中根项点击右键添加Digital object，并填写Display Name，选择Name和Digital object。

​	注意：在切换Domain的Tab前，务必先选中切换前Domain的根项，否则选中项的Digital object属性会丢失。

<img src= "https://ftp.bmp.ovh/imgs/2019/12/f0044f80af2358ac.png" align=center height="300px" width="600px"/>

​	配置好后点击`Finish`按钮。

### 配置完成

​	配置成功后会弹出对话框如下图。

<img src= "https://i.postimg.cc/vBTWBFzT/success.png" align=center height="300px" width="500px"/>

​	在`.\iS3-Desktop-Client\Output\Data`路径下也会生成一个项目配置文件。在该例中，出现`TONGJI.xml`文件。内容如下：

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

​	项目配置文件也可以自己手动编写。

