#  API配置



​	API配置用于给Client端的访问提供接口并返回数据。

​	接口的配置涉及到了`EntityFramework`的映射（详见[附录：EFCF属性映射约定](../../chapter3/appendix.md)）。

### Geology

​	Geology类型的数据需要在`.\iS3.MiniServer\iS3.MiniServer\GeologyController.cs`中增加API名称和调用方法。

​	以钻孔`Borehole`的API为例：

```cs
//<summary>
//根据id获取工程钻孔数据
//</summary>
//<param name="project">项目名称</param>
//<param name="id">钻孔id</param>
[Route("borehole")]
[HttpGet]
public Borehole getBoreholeById(string project, int id)
{
    var repo = 		RepositoryForServer<Borehole>.GetInstance(project);
    return repo.Retrieve(id).Result;
}
```

​	同时在`.\iS3.MiniServer\iS3.Geology.Server\GeologyContext.cs`中加入获取表内信息的代码，以钻孔的表为例：

```cs
public virtual DbSet<Borehole> Borehole { get; set; }
```

​	在`.\iS3.MiniServer\iS3.Geology\GeologyContext.cs`中加入获取表内信息的代码以及API名称和调用方法，以钻孔为例：

```csharp
public virtual DbSet<Borehole> Borehole { get; set; }
```

```csharp
 //<summary>
// 根据id获取工程钻孔数据
//</summary>
//<param name="project">项目名称</param>
//<param name="id">钻孔id</param>
[Route("borehole")]
[HttpGet]
public Borehole getBoreholeById(string project, int id)
{  //通过getBoreholeById()方法获取数据
    var repo = RepositoryForServer<Borehole>.GetInstance(project);
    return repo.Retrieve(id).Result;
}
```

### Monitoring

​	Monitoring类型的数据需要在`.\iS3.MiniServer\iS3.MiniServer\ MonitoringController.cs`中增加API名称和调用方法。

​	以监测点`MonPoint`的API为例：

```cs
[Route("monpoint")]
[HttpGet]
public List<MonPoint> getMonPointListByObjsID(string project, int objsid, string filter)
{  //通过getMonPointListByObjsID()方法获取数据
    var repo = new RepositoryForServer<MonPoint>(project);
    return repo.RetrieveAll().Result;
}
```

​	在`.\iS3.MiniServer\iS3.Monitoring\MonitoringContext.cs`中加入获取表内部信息的代码，以监测点`MonPoint`的表为例：

```cs
public virtual DbSet<MonPoint> MonPoint { get; set; }
```

