# 目录结构

在`iS3-Desktop-Client / IS3-Extensions`路径下新建一个`IS3-Geology`文件夹，并在其下创建项目。如图所示是该项目的目录。

<img src="./img/client1.png" alt="client1" style="zoom:80%;" />



- `Properties`：该文件夹是新建项目时自动生成的，用来设定生成的有关程序集的常规信息库文件的一些参数。
- `EntryPoint.cs`：该类是拓展库的入口。
- `Borehole.cs`等：这些类是实现具体数据对象的类，继承自`DGObject`类。
- `IS3-Geology.csproj`：该文件是新建项目时生成的C#项目文件。
- 其他：数据对象类功能实现的辅助类。

