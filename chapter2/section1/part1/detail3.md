# 添加项目到数据库



新创建的项目信息需要添加到数据库中，才能在iS3平台访问并使用。

项目信息应被添加到数据库`TS_iS3_V2`的工程列表中（表名为`Project_ProjectLocation`）。

如果数据库中不存在该表，首先应该添加这张表。

建表的DDL如下：

```sql
create table Project_ProjectLocation
(
	#编号，主码
    object_id    int identity
        constraint [PK_dbo.Project_ProjectLocation]
            primary key,
	#项目代号，即为配置文件XML中的项目ID
    CODE         nvarchar(max),
	#项目名称，即为配置文件XML中的项目名称
    ProjectTitle nvarchar(max),
    #横坐标
    X            decimal(18, 2),
    #纵坐标
    Y            decimal(18, 2),
    #项目描述
    Description  nvarchar(max),
    #ID，按顺序添加即可
    ID           int not null,
    Name         nvarchar(max),
    FullName     nvarchar(max)
)
go


```

建表成功后，向表中添加数据。

可以将上一步配置文件时生成的`ProjectList.xml`作为辅助工具，将其中记录的数据一一填入数据库。

如在本例中，生成的`ProjectList.xml`中的`<ProjectList.Locations>`里的这条数据：

```XML
<ProjectList.Locations>
    <ProjectLocation X="13046499.00" Y="3988819.22" ID="TONGJI" DefinitionFile="TONGJI.xml" Description="这是迁移后的数据测试" Default="false" />
  </ProjectList.Locations>
```

添加到数据库后：

![detail3](./img/detail3.png)

添加后，提交更新。