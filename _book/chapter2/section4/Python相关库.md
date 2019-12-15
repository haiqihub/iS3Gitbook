###  Python相关库

#### 1. IS3-Python

放置于..\iS3-Desktop-Client\IS3-Python文件目录下的C#代码库，提供IS3内置的Python功能，包括对IronPythonControl等的实现和控制，一般不必关心。

#### 2. Python扩展插件

放置于..\iS3-Desktop-Client\Output\PyPlugins 文件目录下的*.py脚本文件库。主程序运行时将自动执行位于该路径下的所有Python脚本文件（通过Mainframe.xaml.cs文件中定义的loadPyPlugins函数进行加载）。

iS3中提供了名为plugin-demo.py的测试文件，该样例位于..\\Output\IS3Py目录下。

> 注：初始情况下该路径不存在，需用户手动创建该文件夹。

#### 3. Python脚本库

放置于..\Output\IS3Py\目录下的Python脚本文件。该目录下的脚本文件供用户在使用Python进行二次开发时调用。用户编写的Python文件也应放置于该目录下，使用时以import语句导入该库名称即可。

以iS3.py脚本库为例，该库提供了对addView（）的封装等操作,调用样例可参考TONGJI.py和plugin-demo.py。

> 补充说明：IronPython-2.7.5.msi是iS3 二次开发主要插件，软件开发和发布都需要安装此插件。二次开发内嵌Python语言开发工具，提供Python语言开发范例，提供C#二次开发接口和范例。   

#### 4. Python配置文件

放置于..\\iS3-Desktop-Client\Output\Data\\[project_name]文件目录下的Python脚本文件。该Python脚本可作为iS3工程管理的入口，主要用于初始化工程，关联XML配置文件，导入二维、三维图形以及数据。该文件实际上就是用于加载[project_name]工程的Python脚本，是加载iS3工程项目的另一种方式（还有一种就是在iS3-Desktop里通过C#加载 ）。

以TONGJI的工程项目为例，Python配置文件格式如下所示。其中，LoadPrj() 函数用于加载工程及其相应XML文件。该py文件在使用cong.exe配置项目时自动配置生成。

 ```python
   # -*- coding:gb2312 -*-
   import is3
   is3.mainframe.LoadProject('TONGJI.xml')
   is3.prj = is3.mainframe.prj
   is3.MainframeWrapper.loadDomainPanels()
   for emap in is3.prj.projDef.EngineeringMaps:
       is3.MainframeWrapper.addView(emap)
   is3.addView3d('Map3D', 'TONGJI.unity3d')
 ```

#### 5. 原readme.txt附件

此处附上原readme文件（略有修正）。

> **readme.txt**
>
> (1) Scripts can be loadded and runned in iS3. In this case, the scripts is runned in the main UI thread. Note that script call to IS3View.addGdbLayer will hang the program (load tt. py will hang the program). This problem doesn't exist in the following case.
>
> (2) Scripts can be inputted and runned immediately in the Python console window. In this case, the scripts is runned in another thread which is different from the main UI thread. In Windows, UI thread vars and functions are restricted to other threads. So, be caution with script calls to functions in UI thread. Classes in the main UI thread include: mainframe, view, layer, etc.
>
> (3) is3. py provides some friendly classes and vars to facilitate use of iS3, such as adding views and layers. Please note that wrapper classes to mainframe, view, and layer are provided to overcome the restrictions of calls to the main UI thread. The wrapper classes are also called thread safe classes.
>
> (4) Scripts located in /IS3/bin/PyPlugins/(修正：实际应为../Output/PyPlugins)  will be automatically runned when program starts. It is a nice place to put your frequent use scripts here, such as user-defined toolboxes. See plugin-demo. py for more details.

