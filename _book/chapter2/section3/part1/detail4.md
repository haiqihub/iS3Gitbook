# 运行配置

## 1. 生成解决方案

此处应点击上方绿色箭头，生成相应项目的解决方案。

<div style= text-align:center>
<img src="..\img\client4.png" style='width:600px'; 'left: 50%' />
</div>

生成情况如下图所示：

<div style= text-align:center>
<img src="..\img\client5.png" style='width:600px'; 'left: 50%' />
</div>


## 2. 修改dll文件路径

如第二小节（定义扩展类入口）所述，扩展类生成的dll文件必须放置在`\bin\extensions`目录下，才能被`ExtensionManager`类读取。但默认情况下生成的dll文件路径如上图所示(Debug模式)。因此，我们必须将生成的扩展类的dll文件和pdb文件移动到`iS3-Desktop-Client\bin\extensions`目录下。

也可以选择在生成解决方案之前，在csproj中对路径进行修改，此处不再附图。

<div style= text-align:center>
<img src="..\img\client6.png"  style='width:600px'; 'left: 50%' />
</div>

> 注意：在Visual Studio 2017的环境下可能会出现无法启动程序的弹窗，但控制台会显示是否生成成功。

## 3. 启动项目

启动`iS3-Desktop.csproj`，你将看到运行成功的IS3桌面应用程序。

<div style= text-align:center>
<img src="..\img\client7.png" style='width:600px'; 'left: 50%' />
</div>