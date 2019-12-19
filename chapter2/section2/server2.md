#  API配置



API配置用于给Client端的访问提供接口并返回数据。



### Geology

**1**.` Geology`类型的数据需要在`.\iS3.MiniServer\iS3.MiniServer\GeologyController.cs`中增加API名称和调用方法。

以钻孔`borehole`的API为例：

①通过`getBoreholeById()`方法，根据`id`获取工程钻孔数据

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
    var repo = 	RepositoryForServer<Borehole>.GetInstance(project);
    return repo.Retrieve(id).Result;
}
```

②通过`getBoreholeByObjs()`方法，根据对象组获取工程钻孔数据

```cs
/// <summary>
/// 根据对象组获取钻孔
/// </summary>
/// <param name="project"></param>
/// <param name="objsid"></param>
/// <param name="filter"></param>
/// <returns></returns>
[Route("borehole")]
[HttpGet]
public List<Borehole> getBoreholeByObjs(string project, int objsid, string filter)
{
var repo = RepositoryForServer<Borehole>.GetInstance(project);
return repo.RetrieveByObjs(objsid, filter).Result;
}
[Route("borehole")]
[HttpGet]
public List<Borehole> getBoreholeList(string project)
{
 var repo = new RepositoryForServer<Borehole>(project);
 return repo.RetrieveAll().Result;
 }
```
③通过`postBorehole()`方法，新增钻孔对象

```cs
/// <summary>
/// 新增borehole对象,连带其中的钻孔地层对象，如果有的话
/// </summary>
/// <param name="project"></param>
/// <param name="model"></param>
/// <returns></returns>
[Route("borehole")]
[HttpPost]
public Borehole postBorehole([FromUri]string project, [FromBody]Borehole model)
{
    var repo = RepositoryForServer<Borehole>.GetInstance(project);
    return repo.Create(model).Result;
}
```

④通过`putBorehole()`方法，更新钻孔信息

```cs
/// <summary>
/// 更新钻孔信息，不更新其中的钻孔地层对象信息
/// </summary>
/// <param name="project"></param>
/// <param name="model"></param>
/// <returns></returns>
[Route("borehole")]
[HttpPut]
public Borehole putBorehole([FromUri]string project, [FromBody]Borehole model)
{
    var repo = RepositoryForServer<Borehole>.GetInstance(project);
    return repo.Update(model).Result;
}
```

⑤通过`deleteBorehole()`方法， 删除钻孔对象

```cs
/// <summary>
/// 删除钻孔对象，同时删除对应的钻孔地层对象
/// </summary>
/// <param name="project"></param>
/// <param name="model"></param>
/// <returns></returns>
[Route("borehole")]
[HttpDelete]
public int deleteBorehole([FromUri]string project, [FromBody]Borehole model)
{
    var repo = RepositoryForServer<Borehole>.GetInstance(project);
    return repo.Delete(model).Result;
}
```

**2**.在`.\iS3.MiniServer\iS3.Geology.Server\GeologyContext.cs`中加入获取表内信息的代码，以钻孔的表为例：

```cs
public virtual DbSet<Borehole> Borehole { get; set; }
```

**3**.在`.\iS3.MiniServer\iS3.Geology\GeologyContext.cs`中加入获取表内信息的代码，以钻孔为例：

```csharp
public virtual DbSet<Borehole> Borehole { get; set; }
```

以及API名称和调用方法，以`getBoreholeById()`方法为例：

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

**1**.`Monitoring`类型的数据需要在`.\iS3.MiniServer\iS3.MiniServer\ MonitoringController.cs`中增加API名称和调用方法。

以监测点`monpoint`的API为例：

①通过`getMonPointListByObjsID()`方法，根据`objsid`获取工程监测点数据

```cs
[Route("monpoint")]
[HttpGet]
public List<MonPoint> getMonPointListByObjsID(string project, int objsid, string filter)
{  
    var repo = new RepositoryForServer<MonPoint>(project);
    return repo.RetrieveAll().Result;
}
     
```

②通过`getMonPointByID()`方法，根据`id`获取工程监测点数据

```cs
[Route("monpoint")]
[HttpGet]
public MonPoint getMonPointByID(string project, int id)
{
    var repo = new RepositoryForServer<MonPoint>(project);
    MonitoringContext monContext = repo.context as MonitoringContext;
    MonPoint mp = monContext.MonPoint.Where(x => x.ID == id).FirstOrDefault();
    if (mp == null) return null;
	mp.monComponentList = new List<MonComponent>();
	List<string> componentNameList = mp.componentNames.Split(',').ToList();
	foreach (string key in componentNameList)
{
    mp.monComponentList.Add(new MonComponent()
    {
        componentName = key,
        readings = monContext.MonReading.Where(x => ((x.monPointName == mp.Name) && (x.component == key))).ToList()
    });
}
return mp;
} 
```

③通过`postMonPoint()`方法，新增监测点对象

```cs
/// <param name="project"></param>
/// <param name="model"></param>
/// <returns></returns>
[Route("monpoint")]
[HttpPost]
public MonPoint postMonPoint([FromUri]string project, [FromBody]MonPoint model)
{
    var repo = RepositoryForServer<MonPoint>.GetInstance(project);
    return repo.Create(model).Result;
}

```

④通过`putMonPoint()`方法，更新监测点信息

```cs
/// <param name="project"></param>
/// <param name="model"></param>
/// <returns></returns>
[Route("monpoint")]
[HttpPut]
public MonPoint putMonPoint([FromUri]string project, [FromBody]MonPoint model)
{
    var repo = RepositoryForServer<MonPoint>.GetInstance(project);
    return repo.Update(model).Result;
}
```

⑤通过`deleteMonPoint()`方法， 删除监测点对象

```cs
/// <param name="project"></param>
/// <param name="model"></param>
/// <returns></returns>
[Route("monpoint")]
[HttpDelete]
public int deleteMonPoint([FromUri]string project, [FromBody]MonPoint model)
{
    var repo = RepositoryForServer<MonPoint>.GetInstance(project);
    return repo.Delete(model).Result;
}
```

**2**.在`.\iS3.MiniServer\iS3.Monitoring\MonitoringContext.cs`中加入获取表内部信息的代码，以监测点`MonPoint`的表为例：

```cs
public virtual DbSet<MonPoint> MonPoint { get; set; }
```

