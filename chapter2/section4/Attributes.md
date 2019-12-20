### 1. Attribute

is3脚本库中直接封装了程序运行时的一些全局变量，为用户提供了更为灵活的开发空间。由于UI 线程变量以及函数被限制在其他线程当中，因此，在UI线程中使用Python调用函数时应分外小心。直接调用is3脚本库编写函数时需注意使用`delegate`方法。

#### 1.1 属性名称

| 序号 |    属性名称    |    属性类型     |     简要说明      |
| :--: | :------------: | :-------------: | :---------------: |
|  1   |   mainframe    |   IMainFrame    |   UI的主要框架    |
|  2   |      prj       |     Project     |     项目工程      |
|  3   |   dispatcher   |   Dispatcher    | mainframe的分发器 |
|  4   | graphicsEngine | IGraphicEngine  |  graphics engine  |
|  5   | geometryEngine | IGeometryEngine |     几何引擎      |

#### 1.2 属性定义

源代码定义如下：

```python
mainframe = Globals.mainframe            # Global var: mainframe
prj = mainframe.prj                      # Global var: prj
dispatcher = mainframe.Dispatcher        # Global var: dispatcher -> UI thread manager
graphicsEngine = Runtime.graphicEngine   # Global var: graphics Engine
geometryEngine = Runtime.geometryEngine  # Global var: geometry Engine
```

#### 1.3 属性说明

##### 1.3.1 mainframe

属性 `mainframe` 对应主程序的全局变量`mainframe`，相当于 MVC 架构中Controller，用于控制应用程序的流程，处理事件并做出响应。

##### 1.3.2 prj

属性 `prj` 对应主程序的全局变量`prj`， 对应当前打开的工程项目，相当于MVC架构中的Model，用于封装与工程项目（`Project`）业务逻辑相关的数据和处理数据的方法。

##### 1.3.3 dispatcher

属性 `dispatcher`  是主程序`mainframe`框架下的分发器，用于任务的分发。

##### 1.3.4 graphicsEngine

属性 `graphicsEngine` 对应主程序 `Runtime` 机制下的图形引擎，用于图形界面的处理。

##### 1.3.4 geometryEngine

属性 `geometryEngine`对应主程序 `Runtime` 机制下的几何引擎，用于几何图形界面的处理。

