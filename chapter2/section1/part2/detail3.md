# server端的配置



​	在.\iS3.MiniServer\iS3.MiniServer\App.config中添加新加项目的数据库信息。

​	以TONGJI项目为例，在< connectionStrings >里添加TONGJI项目的数据库信息。

​	本例中，TS_iS3_V2 数据库中定义了Project的信息(含义参考[平台架构](./../../../chapter1/section1.md)中的数据组织)，其中Project_ProjectLocation表记录了项目列表。具体内容可以在完成本地数据文件配置后参考[添加项目到数据库](./../part1/detail3.md)。

TS_iS3_V2_Test 数据库中定义了不同DGObject的信息(含义参考[平台架构](./../../../chapter1/section1.md)中的数据组织)。



```c#
<connectionStrings>
    <add name="iS3Db" connectionString="server=139.***.**.***; Database=TS_iS3_V2; User ID=***; Password=******" providerName="System.Data.SqlClient" />
    <add name="iS3DemoTest" connectionString="server=139.***.**.***; Database=TS_iS3_V2_Test; User ID=***; Password=******" providerName="System.Data.SqlClient" />
    <add name="TONGJI" connectionString="server=139.***.**.***; Database=TS_iS3_V2_Test; User ID=***; Password=******" providerName="System.Data.SqlClient" />
  </connectionStrings>
```

