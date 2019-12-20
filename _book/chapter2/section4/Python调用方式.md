### Python调用方式

此处将介绍调用Python脚本文件的3种途径，包括`IronPython`控制面板，`IronPython Pad`面板，以及Python扩展插件库，并对其适用场景和使用方法进行简单介绍。

#### 1. IronPython Console

`IronPython`控制面板如下图所示，此处可实时执行Python代码，适用于实时的、简短直接的调用或测试。注意。供调用的Python脚本库应放置在`..\Out\IS3Py\`目录下。

>  理论上位于`.. \Output\PyPlugins`路径下的Python脚本库也会被iS3识别，但由于位于该目录下的脚本文件在主程序运行时就会被自动执行，不适用于上述场景。

<div style= text-align:center>
<img src=".\IronPythonConsole.png"  style='width:600px'; 'left: 50%' />
</div>

此处，我们将对**Console**的使用进行简单示范。

测试用例：

```python
>>> import is3
>>> is3.addView3d('Map3D', 'TONGJI.unity3d')
﻿<is3.ViewWrapper instance at 0x000000000000002B>
```

测试结果：

工程图形展示面板多了一层名为`Map 3D`的3D图层。

<div style= text-align:center>
<img src=".\1.png"  style='width:600px'; 'left: 50%' />
</div>

#### 2. IronPython Pad

`IronPython Pad`面板如下所示，此处可加载并执行Python脚本文件，可点击文件图标打开指定路径下的文件加载脚本，也可以直接在该面板编写长段代码。执行代码请点击绿色箭头图标。

<div style= text-align:center>
<img src=".\IronPythonPad.png"  style='width:600px'; 'left: 50%' />
</div>

#### 3.  Python扩展插件

如第一小节所述，位于`.. \Output\PyPlugins`目录下的所有python文件都会在加载工程时被自动执行。

以`plugin-demo.py`为例，将`plugin-demo.py`脚本文件放置于该目录下，加载工程时自动执行该脚本，将`Demo`加至**Tools**面板。点击该**Demo**字样时，系统会在图形展示面板添加一层名为`Demo`的3D图层，

<div style= text-align:center>
<img src=".\Tools.png"  style='width:600px'; 'left: 50%' />
</div>