# server端的配置



在`.\iS3.MiniServer\iS3.MiniServer\App.config`中添加新加项目的数据库信息。

以`TONGJI`项目为例，在<connectionStrings>里添加`TONGJI`项目的数据库信息。



```c#
<connectionStrings>
    <add name="iS3Db" connectionString="server=139.***.**.***; Database=TS_iS3_V2; User ID=***; Password=******" providerName="System.Data.SqlClient" />
    <add name="iS3DemoTest" connectionString="server=139.***.**.***; Database=TS_iS3_V2_Test; User ID=***; Password=******" providerName="System.Data.SqlClient" />
    <add name="TONGJI" connectionString="server=139.***.**.***; Database=TS_iS3_V2_Test; User ID=***; Password=******" providerName="System.Data.SqlClient" />
  </connectionStrings>
```



本例中：

`TS_iS3_V2` 数据库中定义了Project的信息(含义参考[平台架构(一、平台说明/第一章 平台架构)](./../../../chapter1/section1.md)中的数据组织)，其中的`Project_ProjectLocation`表包含所有可以在iS3平台加载的项目简要信息，如项目名称、ID、描述等（此处大致理解，具体内容可以在完成[本地数据配置(二、平台使用/第一章 数据准备/第二节 本地数据配置)](./../part1.md)步骤后参考[添加项目到数据库(二、平台使用/第一章 数据准备/第二节 本地数据配置/3. 添加项目到数据库)](./../part1/detail3.md)）。

`TS_iS3_V2_Test` 数据库中定义了不同`DGObject`的信息(含义参考[平台架构(一、平台说明/第一章 平台架构)](./../../../chapter1/section1.md)中的数据组织)。