# 数据文件准备



本小节涉及的数据类型的介绍详见[平台架构(一、平台说明/第一章 平台架构)](./../../../chapter1/section1.md)中的**iS3数据层架构**的内容。

## step1

在`.\iS3-Desktop-Client\Output\Data`路径下新建名为项目名称的文件夹，并在该文件夹下加入`geodatabase` ,`py`,`unity3d`三个数据文件。

`geodatabase`文件是地理数据库`Geodatabase`的文件。地理数据库是一种面向对象的空间数据模型。在iS3平台中该文件用于工程2D模型数字化对象的存储。iS3平台将数字化对象与数据库存储的数据通过数字化对象的ID进行绑定。

`py`文件是python的脚本文件。在iS3平台中该文件用于二次开发。

`unity3d`文件游戏引擎Unity3D的文件。Unity3D可以用于三维建筑可视化开发。在iS3平台中，该文件用于工程3D模型数据的存储。

以`TONGJI`项目为例，在`.\iS3_2.0_Demo-master\iS3-Desktop-Client\Output\Data`路径下新建`TONGJI`的文件夹，将在该文件夹下加入`tongji.geodatabase`,`TONGJI.py`,`tongji.unity3d`三个数据文件（上述三个文件可以通过[平台使用](./../../README.md)中的链接下载数据文件获取）。



<div style= text-align:center>
<img src= "https://i.postimg.cc/hPNgZ9w5/data.png" style='width:600px'; 'left: 50%'/>
</div>

`tongji.geodatabase`里存储了工程的钻孔、监测点的数字化对象。

`TONGJI.py`里通过调用iS3平台封装好的接口来加载项目。

`tongji.unity3d`里存储了工程的地铁站的3D信息。

## step2

在`.\iS3-Desktop-Client\Output\Data\TPKs`文件夹下加入`tpk`文件。

`tpk`文件`ArcGIS`的一种数据文件类型，主要用于将切片文件打包成离线地图包。在iS3平台中该文件用于存储项目的地图。

以`TONGJI`项目为例，在`.\iS3-Desktop-Client\Output\Data\TPKs`文件夹下加入`tongji.tpk`文件（该文件也可通过[平台使用](./../../README.md)中的链接下载数据文件获取）。

`TONGJI.tpk`里存储了同济地铁车站附近的地图。

<div style= text-align:center>
<img src= "https://i.postimg.cc/CLq919Pc/tpks.png"  style='width:600px'; 'left: 50%'/>
</div>

## step3

运行`iS3.Config.exe`。