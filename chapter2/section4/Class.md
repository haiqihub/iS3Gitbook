### 2. Class

为了实现UI线程中函数的安全调用，is3脚本库中定义了了一些类，将Mainframe，View中的函数以Wrapper的形式进行包装。

#### 2.1 MainframeWrapper

MainframeWrapper类下目前有两个方法，且均为静态方法，可直接调用。

##### 2.1.1 addView

**接口功能**

静态方法，添加新的视图（View）。封装在MainrameWrapper类中，调用了Mainframe.xaml.cs中的addView（）函数。

**接口参数**

| 序号 |   名称   |      类型      | 是否必须 | 示例值 |       描述       |
| :--: | :------: | :------------: | :------: | :----: | :--------------: |
|  1   |   emap   | EngineeringMap |    是    |   无   | 需添加的工程地图 |
|  2   | canClose |      bool      |    否    |   无   |    默认为true    |

**返回结果**

|    类型     | 示例值 |        描述        |
| :---------: | :----: | :----------------: |
| ViewWrapper |   无   | 返回新View的包装类 |

**调用示例**

```python
emap = is3.EngineeringMap('demo', 0, 0, 100, 100, 0.01)
safe_view = is3.MainframeWrapper.addView(emap)
```

##### 2.1.2 loadDomainPanels

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



#### 2.2 ViewWrapper

ViewWrapper中的addGdbLayer()和addShpLayer()方法一般不直接调用，添加GdbLayer和ShpLayer时应调用is3下封装好的addGdbLayer()和addShpLayer()方法。

##### 2.2.1  \_init\_

**接口功能**

初始化ViewWrapper

**接口参数**

| 序号 | 名称 | 类型  | 是否必须 | 示例值 |     描述      |
| :--: | :--: | :---: | :------: | :----: | :-----------: |
|  1   | view | IView |    是    |   无   | graphics 图层 |

**返回结果**

|    类型     | 示例值 |       说明        |
| :---------: | :----: | :---------------: |
| ViewWrapper |   无   | 返回IView的包装类 |

**调用示例**

无

##### 2.2.2 addLayer

**接口功能**

添加新的图层（Layer）。封装在ViewWrapper类中，调用了View.cs中的addLayer()函数。

**接口参数**

| 序号 | 名称  |      类型      | 是否必须 | 示例值 |     描述      |
| :--: | :---: | :------------: | :------: | :----: | :-----------: |
|  1   | layer | IGraphicsLayer |    是    |   无   | graphics 图层 |

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

##### 2.2.3 addLocalTiledLayer

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

##### 2.2.4 addGdbLayer

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

##### 2.2.5 addShpLayer

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

##### 2.2.6 selectByRect

**接口功能**

在iS3程序中，对应图层左上方的紫色矩形“select objects on the map”的触发。该函数封装在ViewWrapper类中，调用了IS3View.cs中的 selectByRect（）函数。

该函数调用尚不明确，功能尚不完善。



#### 2.3 GraphicsLayerWrapper

##### 2.3.1   \_init\_

**接口功能**

初始化GraphicsLayerWrapper。

**接口参数**

| 序号 |  名称  |       类型       | 是否必须 | 示例值 |        描述         |
| :--: | :----: | :--------------: | :------: | :----: | :-----------------: |
|  1   | glayer | IS3GraphicsLayer |    是    |   无   | 需包装的Graphic图层 |

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

##### 2.3.2 setRenderer

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

##### 2.2.3 addGraphic

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