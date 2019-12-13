### iS3脚本库说明

该项目中..\Output\IS3Py 目录下放置了以i3.py为主、供用户调用的Python脚本库。plugin-demo.py, graphics-demo.py都是调用is3库封装的函数以实现功能的范例。用户编写的python脚本文件也可放置在该目录下，供IronPythonConsole面板调用。

本文档将介绍is3.py脚本库中的所有函数，供用户对照使用。

#### 1. Attributes

is3脚本库中直接封装了程序运行时的一些全局变量，为用户提供了更为灵活的开发空间。由于UI 线程变量以及函数被限制在其他线程当中，因此，在UI线程中使用Python调用函数时应分外小心。直接调用is3脚本库编写函数时需注意使用delegate方法。

##### 1.1 属性名称

| 序号 |    属性名称    |    属性类型     |            说明             |
| :--: | :------------: | :-------------: | :-------------------------: |
|  1   |   mainframe    |   IMainFrame    |      the UI main frame      |
|  2   |      prj       |     Project     |        project data         |
|  3   |   dispatcher   |   Dispatcher    | the Dispatcher of mainframe |
|  4   | graphicsEngine | IGraphicEngine  |       graphics engine       |
|  5   | geometryEngine | IGeometryEngine |       gemoetry engine       |

##### 1.2 属性定义

```python
mainframe = Globals.mainframe            # Global var: mainframe
prj = mainframe.prj                      # Global var: prj
dispatcher = mainframe.Dispatcher        # Global var: dispatcher -> UI thread manager
graphicsEngine = Runtime.graphicEngine   # Global var: graphics Engine
geometryEngine = Runtime.geometryEngine  # Global var: geometry Engine
```



#### 2. Functions

is3脚本库中封装了一些C#里的函数，可满足用户添加图层的基本需求。为了实现UI线程中函数的安全调用，Mainframe，View中的函数以Wrapper的形式进行包装。如果用户想要更多的功能性拓展，仿照addView（）等函数的写法，调用C#中的函数即可。

##### 2.1 MainframeWrapper

MainframeWrapper类下目前有两个方法，且均为静态方法，可直接调用。

###### 2.1.1 addView

**接口功能**

静态方法，添加新的视图（View）。封装在MainrameWrapper类中，调用了Mainframe.xaml.cs中的addView（）函数。

**接口参数**

| 序号 |   名称   |      类型      | 是否必须 | 示例值 |          描述           |
| :--: | :------: | :------------: | :------: | :----: | :---------------------: |
|  1   |   emap   | EngineeringMap |    是    |   无   | 需添加的engineering map |
|  2   | canClose |      bool      |    否    |   无   |       默认为true        |

**返回结果**

|    类型     | 示例值 |        描述        |
| :---------: | :----: | :----------------: |
| ViewWrapper |   无   | 返回新View的包装类 |

**调用示例**

```python
emap = is3.EngineeringMap('demo', 0, 0, 100, 100, 0.01)
safe_view = is3.MainframeWrapper.addView(emap)
```

###### 2.1.2 loadDomainPanels

**接口功能**

静态方法，加载Domain面板，应用于加载工程项目时。封装在MainrameWrapper类中，调用Mainframe.xaml.cs中的loadDomainPanels()函数。

**接口参数**

无

**返回结果**

void

**调用示例**

```python
import is3
is3.MainframeWrapper.loadDomainPanels()
```



##### 2.2 ViewWrapper

ViewWrapper中的addGdbLayer()和addShpLayer()方法一般不直接调用，添加GdbLayer和ShpLayer时应调用is3下封装好的addGdbLayer()和addShpLayer()方法。

###### 2.2.1  \_init\_

**接口功能**

初始化ViewWrapper

**接口参数**

| 序号 | 名称 | 类型  | 是否必须 | 示例值 |      描述      |
| :--: | :--: | :---: | :------: | :----: | :------------: |
|  1   | view | IView |    是    |   无   | graphics layer |

**返回结果**

|    类型     | 示例值 |       说明        |
| :---------: | :----: | :---------------: |
| ViewWrapper |   无   | 返回IView的包装类 |

**调用示例**

无

###### 2.2.2 addLayer

**接口功能**

添加新的图层（Layer）。封装在ViewWrapper类中，调用了View.cs中的addLayer()函数。

**接口参数**

| 序号 | 名称  |      类型      | 是否必须 | 示例值 |      描述      |
| :--: | :---: | :------------: | :------: | :----: | :------------: |
|  1   | layer | IGraphicsLayer |    是    |   无   | graphics layer |

**返回结果**

void

**调用示例**

```python
import is3
emap = is3.EngineeringMap('demo', 0, 0, 100, 100, 0.01)
safe_view = is3.MainframeWrapper.addView(emap)
layer3WP = is3.newGraphicsLayer('layer3', 'layer3')
safe_view.addLayer(layer3WP.layer)
```

###### 2.2.3 addLocalTiledLayer

**接口功能**

添加本地的Tiled Layer文件，文件格式为.TPK。该函数封装在ViewWrapper类中，调用了View.cs中的addLocalTiledLayer()函数。

**接口参数**

| 序号 | 名称 |  类型  | 是否必须 |   示例值    |        描述        |
| :--: | :--: | :----: | :------: | :---------: | :----------------: |
|  1   | file | string |    是    | “Empty.tpk” | .TPK文件的绝对路径 |
|  2   |  id  | string |    是    | 'baselayer' |  layer的唯一标识   |

**返回结果**

void

**调用示例**

```python
import is3
# tilePath: path to tiled packages (.TPK)
tilefile = is3.Runtime.tilePath + "\\Empty.tpk"   
safe_view.addLocalTiledLayer(tilefile, 'baselayer')
```

###### 2.2.4 addGdbLayer

**接口功能**

从本地的geodatabase中动态加载Gdb图层(GdbLayer)。该函数封装在ViewWrapper类中，调用了View.cs中的addGdbLayer（）函数。

**接口参数**

| 序号 |    名称     |   类型   | 是否必须 | 示例值 |                   描述                    |
| :--: | :---------: | :------: | :------: | :----: | :---------------------------------------: |
|  1   |  layerDef   | LayerDef |    是    |   无   |             layer definition              |
|  2   |   gdbFile   |  string  |    是    |   无   |             geodatabase文件名             |
|  3   |    start    |   int    |    否    |   无   |    layer中feature的起始索引，默认为 0     |
|  4   | maxFeatures |   int    |    否    |   无   | 加载layer时所允许的最大features，默认为 0 |

**返回结果**

|         类型         | 示例值 |            描述             |
| :------------------: | :----: | :-------------------------: |
| GraphicsLayerWrapper |   无   | 返回新GraphicsLayer的包装类 |

**调用示例**

```python
import is3
layerWrapper = viewWrapper.addGdbLayer(layerDef, gdbFile, start, maxFeatures)
```

###### 2.2.5 addShpLayer

**接口功能**

从本地的shape文件中动态加载Shp图层(ShpLayer)。该函数封装在ViewWrapper类中，调用了View.cs中的 addShpLayer（）函数。

**接口参数**

| 序号 |    名称     |   类型   | 是否必须 | 示例值 |                   描述                    |
| :--: | :---------: | :------: | :------: | :----: | :---------------------------------------: |
|  1   |  layerDef   | LayerDef |    是    |   无   |             layer definition              |
|  2   |   shpFile   |  string  |    是    |   无   |             shape file文件名              |
|  3   |    start    |   int    |    否    |   无   |    layer中feature的起始索引，默认为 0     |
|  4   | maxFeatures |   int    |    否    |   无   | 加载layer时所允许的最大features，默认为 0 |

**返回结果**

|         类型         | 示例值 |            描述             |
| :------------------: | :----: | :-------------------------: |
| GraphicsLayerWrapper |   无   | 返回新GraphicsLayer的包装类 |

**调用示例**

```python
import is3
layerWrapper = viewWrapper.addShpLayer(layerDef, shpfile, start, maxFeatures)
```

###### 2.2.6 selectByRect

**接口功能**

在iS3程序中，对应图层左上方的紫色矩形“select objects on the map”的触发。该函数封装在ViewWrapper类中，调用了IS3View.cs中的 selectByRect（）函数。

该函数调用尚不明确，功能尚不完善。



##### 2.3 GraphicsLayerWrapper

###### 2.3.1   \_init\_

**接口功能**

初始化GraphicsLayerWrapper。

**接口参数**

| 序号 |  名称  |       类型       | 是否必须 | 示例值 | 描述 |
| :--: | :----: | :--------------: | :------: | :----: | :--: |
|  1   | glayer | IS3GraphicsLayer |    是    |   无   |  需包装的Graphic图层 |

**返回结果**

|         类型         | 示例值 |             说明             |
| :------------------: | :----: | :--------------------------: |
| GraphicsLayerWrapper |   无   | 返回IS3GraphicsLayer的包装类 |

**调用示例**

```python
import is3
layer = graphicsEngine.newGraphicsLayer(id, displayName)
layerWrapper = GraphicsLayerWrapper(layer)
```

###### 2.3.2 setRenderer

**接口功能**

设置Render。该函数封装在GraphicsLayerWrapper类中，调用了IS3Layer.cs中的 setRenderer()函数。

**接口参数**

| 序号 |   名称   |   类型    | 是否必须 | 示例值 |         描述         |
| :--: | :------: | :-------: | :------: | :----: | :------------------: |
|  1   | renderer | IRenderer |    是    |   无   | 需设置的Renderer目标 |

**返回结果**

void

**调用示例**

```python
import is3
renderer1 = is3.graphicsEngine.newSimpleRenderer(sym_point)
layer1WP = is3.newGraphicsLayer('layer1', 'layer1')
layer1WP.setRenderer(renderer1)
```

###### 2.2.3 addGraphic

**接口功能**

添加Graphic图层。该函数封装在GraphicsLayerWrapper类中，调用了IS3Layer.cs中的 addGraphic（）函数。

**接口参数**

| 序号 |  名称   |   类型   | 是否必须 | 示例值 |        描述         |
| :--: | :-----: | :------: | :------: | :----: | :-----------------: |
|  1   | graphic | IGraphic |    是    |   无   | 需添加的Graphic图层 |

**返回结果**

void

**调用示例**

```python
import is3
line = is3.graphicsEngine.newLine(x1, y1, x2, y2, sr)
myLayer.addGraphic(line)
```



##### 2.4 其他函数

###### 2.4.1 newGraphicsLayer

**接口功能**

新建graphics layer层。该函数调用了IS3GraphicEngine.cs中的 newGraphicsLayer（）函数。

**接口参数**

| 序号 |    名称     |  类型  | 是否必须 | 示例值 | 描述 |
| :--: | :---------: | :----: | :------: | :----: | :--: |
|  1   |     id      | string |    是    |   无   |  无  |
|  2   | displayName | string |    是    |   无   |  无  |

**返回结果**

|         类型         | 示例值 |              说明              |
| :------------------: | :----: | :----------------------------: |
| GraphicsLayerWrapper |   无   | 返回新IS3GraphicsLayer的包装类 |

**调用示例**

```python
import is3
layer1WP = is3.newGraphicsLayer('layer1', 'layer1')
```

###### 2.4.2 addView3d

**接口功能**

添加3D视图。调用了MainframeWrapper中的 addView()函数。

**接口参数**

| 序号 | 名称 |  类型  | 是否必须 | 示例值 |       描述       |
| :--: | :--: | :----: | :------: | :----: | :--------------: |
|  1   |  id  | string |    是    |   无   | Map 的唯一标识符 |
|  2   | file | string |    是    |   无   |  本地3D文件路径  |

**返回结果**

|    类型     | 示例值 |         描述         |
| :---------: | :----: | :------------------: |
| ViewWrapper |   无   | 返回新3D视图的包装类 |

**调用示例**

```python
import is3
is3.addView3d('Map3D', 'TONGJI.unity3d')
```

###### 2.4.3 addGdbLayer

**接口功能**

动态加载Gdb图层(GdbLayer)，调用了ViewWrapper中的addGdbLayer（）函数。添加GdbLayer调用该函数而非ViewWrapper中的addGdbLayer（）函数。

**接口参数**

| 序号 |    名称     |    类型     | 是否必须 | 示例值 |         描述          |
| :--: | :---------: | :---------: | :------: | :----: | :-------------------: |
|  1   | viewWrapper | ViewWrapper |    是    |   无   |   需加载的目标视图    |
|  2   |  layerDef   |  LayerDef   |    是    |   无   |   layer  definition   |
|  3   |   gdbFile   |   string    |    否    |   无   | gdb文件路径，默认None |
|  4   |    start    |     int     |    否    |   无   |         默认0         |
|  5   | maxFeatures |     int     |    否    |   无   |         默认0         |

**返回结果**

|         类型         | 示例值 |        描述        |
| :------------------: | :----: | :----------------: |
| GraphicsLayerWrapper |   无   | 返回新图层的包装类 |

**调用示例**

```python
import is3
strLayerWP = is3.addGdbLayer(viewWP, layerDef)
```

###### 2.4.4 addGdbLayerLazy

**接口功能**

新建指定类型的空白Gdb图层(GdbLayer)。调用了addGdbLayer（）函数。与2.6区别：加载和新建。

**接口参数**

| 序号 |    名称     |    类型     | 是否必须 | 示例值 |         描述          |
| :--: | :---------: | :---------: | :------: | :----: | :-------------------: |
|  1   |    view     | ViewWrapper |    是    |   无   |   需加载的目标视图    |
|  2   |    name     |   string    |    是    |   无   |       图层名称        |
|  3   |    type     |   string    |    是    |   无   |       图层类型        |
|  4   |   gdbFile   |   string    |    否    |   无   | Gdb文件路径，默认None |
|  5   |    start    |     int     |    否    |   无   |         默认0         |
|  6   | maxFeatures |     int     |    否    |   无   |         默认0         |

**返回结果**

|         类型         | 示例值 |        描述        |
| :------------------: | :----: | :----------------: |
| GraphicsLayerWrapper |   无   | 返回新图层的包装类 |

**调用示例**

```python
import is3
is3.addGdbLayerLazy(viewWP, 'MON_WAT', is3.GeometryType.Point)
```

###### 2.4.5 addShpLayer

**接口功能**

从本地的shape文件中动态加载Shp图层(ShpLayer)。调用了ViewWrapper中的 addShpLayer（）函数。添加ShpLayer时调用该函数而非ViewWrapper中的addShpLayer（）函数。

**接口参数**

| 序号 |    名称     |    类型     | 是否必须 | 示例值 | 描述  |
| :--: | :---------: | :---------: | :------: | :----: | :---: |
|  1   | viewWrapper | ViewWrapper |    是    |   无   |  无   |
|  2   |  layerDef   |  LayerDef   |    是    |   无   |  无   |
|  3   |   shpfile   |   string    |    是    |   无   |  无   |
|  4   |    start    |     int     |    否    |   无   | 默认0 |
|  5   | maxFeatures |     int     |    否    |   无   | 默认0 |

**返回结果**

|         类型         | 示例值 |        描述        |
| :------------------: | :----: | :----------------: |
| GraphicsLayerWrapper |   无   | 返回新图层的包装类 |

**调用示例**

```python
import is3
bkgLayerWP = is3.addShpLayer(viewWP, layerDef, file, start, maxFeatures)
```



