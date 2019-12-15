# 表的创建



###  iS3AreaHandle类

​	iS3平台设定每张表的实体类都继承 .\iS3.MiniServer\iS3.Core\iS3Core.cs文件中的 `iS3AreaHandle`类，换言之，在不改变这个类的内容的情况下，每张表都要有其中的 object_id，ID，Name，Description，FullName 这五列，数据类型也在类`iS3AreaHandle`中有注明，其余列名用户可根据自己的数据对应命名。当然用户也可以根据自己数据的特点对 iS3AreaHandle类进行修改，相应地，表的公共属性也进行对应的改变。

### 建表

​	Entity Framework Code First与数据表之间有两种映射方式（详见[附录：EFCF属性映射约定](../../../chapter3/appendix.md)），选择某一种或两种方式兼用也可，根据不同方式创建表名。

​	在远程数据库与在Server端`App.config`文件（详见[server端的配置](./detail3.md)）里添加的库名一致的数据库中建表并导入与配置文件`geodatabase`相对应的数据。表名应与对应的DGObject在Server端的Model文件里的Table表名保持一致，且表名前缀应与`GeologyContext.cs`文件里的`TablePrefix`保持一致。

​	以borehole为例，Geology_Borehole DDL如下：

```sql
create table Geology_Borehole
( #以下是对borehole属性的定义，左边一列是属性，右边一列是属性的数据类型
    object_id       int           not null,
    ID              int,
    Name            varchar(20)   not null
        constraint Geology_Borehole_pk
            primary key nonclustered,
    FullName        nvarchar(max) not null,
    Description     nvarchar(max),
    StratumSection  int,
    SectionSequence int,
    BoreholeType    nvarchar(max) not null,
    TopElevation    float         not null,
    BoreholeLength  float         not null,
    Mileage         nvarchar(max),
    Xcoordinate     float,
    Ycoordinate     float,
    OBJECTID        nvarchar(max)
)
go
```

