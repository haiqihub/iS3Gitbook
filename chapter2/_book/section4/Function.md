# 3.  Function
这里将is3脚本库定义的类的函数进行进一步封装，可满足用户添加图层的基本需求。如果用户想要更多的功能性拓展，仿照`addView()`等函数的写法，调用C#中的函数即可。

## 3.1 newGraphicsLayer

**接口功能**

新建`graphics layer`层。该函数调用了`IS3GraphicEngine.cs`中的 `newGraphicsLayer()`函数。

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

## 3.2 addView3d

**接口功能**

添加3D视图。调用了`MainframeWrapper`中的 `addView()`函数。

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

## 3.3 addGdbLayer

**接口功能**

动态加载Gdb图层(`GdbLayer`)，调用了`ViewWrapper`中的`addGdbLayer()`函数。添加`GdbLayer`调用该函数而非`ViewWrapper`中的`addGdbLayer()`函数。

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

## 3.4 addGdbLayerLazy

**接口功能**

新建指定类型的空白Gdb图层(`GdbLayer`)。调用了`addGdbLayer()`函数。与2.6区别：加载和新建。

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

## 3.5 addShpLayer

**接口功能**

从本地的shape文件中动态加载Shp图层(`ShpLayer`)。调用了`ViewWrapper`中的 `addShpLayer()`函数。添加`ShpLayer`时调用该函数而非`ViewWrapper`中的`addShpLayer()`函数。

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