# server端的配置



​	在.\iS3.MiniServer\iS3.MiniServer\App.config中添加新加项目的数据库信息。

​	以TONGJI项目为例，在<connectionStrings>里添加TONGJI项目的数据库信息。



```c#
<connectionStrings>
    <add name="iS3Db" connectionString="server=139.***.**.***; Database=TS_iS3_V2; User ID=***; Password=******" providerName="System.Data.SqlClient" />
    <add name="iS3DemoTest" connectionString="server=139.***.**.***; Database=TS_iS3_V2_Test; User ID=***; Password=******" providerName="System.Data.SqlClient" />
    <add name="TONGJI" connectionString="server=139.***.**.***; Database=TS_iS3_V2_Test; User ID=***; Password=******" providerName="System.Data.SqlClient" />
  </connectionStrings>
```

