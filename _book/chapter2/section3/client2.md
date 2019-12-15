# 定义扩展类入口

本小节将引导用户定义一个 `EntryPoint`类。 EntryPoint类是扩展库的入口，继承自iS3.Core中的Extensions类。有了该EntryPoint类，主程序运行时才能识别用户自定义的扩展类。用户自定义的扩展类均需继承该Extensions类，否则主程序无法识别该自定义扩展类。

### 1. Extensions类说明

iS3.Core中的Extensions类是用户实现自定义扩展类所依赖的重要对象。所有用户自定义的扩展类均需继承自该Extensions类，否则无法实现扩展。Extensions类的定义如下所示：

```c#
	public class Extensions
    {
        // Summary:
        //     Name, version and provide of the extension
        public virtual string name() { return "Unknown extension"; }
        public virtual string version() { return "Unknown"; }
        public virtual string provider() { return "Unknown provider"; }

        // Summary:
        //     Initialize the extension, called immediately after loaded.
        // Return value:
        //     A string that will be printed in output window.
        public virtual string init() 
        {
            string msg = String.Format("Loaded {0} by {1}, version {2}.\n",
                name(), provider(), version());
            return msg; 
        }
    }
```

自定义的扩展类继承自Extensions类后，编译生成的动态链接文件应该放置在\bin\extensions目录下，否则无法被主程序识别。iS3.Core中的ExtensionsManager类将读取放置于\bin\extensions目录下的dll文件，从类名中创建出实体。



### 2. 实现EntryPoint类

虽然Extensions的实现涉及比较多的代码和文件。但是定义一个扩展类的入口还是很简单的，此处，我们将实现扩展类的入口——EntryPoint类，并对其函数进行介绍。

以同济大学项目的演示为例，代码如下：

```cs
namespace iS3.Geology
{
    // Summary:
    //     This is the entry point for the extension
    public class EntryPoint : Extensions
    {
        public override string name() { return "iS3.Geology"; }
        public override string provider() { return "Tongji iS3 team"; }
        public override string version() { return "1.0"; }
    }
}
```

- `name()`：返回所处的命名空间。此处所处的`namespace`为`iS3.Geology`，所以应该返回"iS3.Geology"。
- `provider()` ：返回开发团队名称。
- `version()`：返回版本信息。