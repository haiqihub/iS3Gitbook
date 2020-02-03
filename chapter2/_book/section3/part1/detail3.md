# 定义拓展类入口

本小节将对`Extensions`类进行说明，并引导用户定义一个 `EntryPoint`类。 `EntryPoint`类是扩展库的入口，继承自`iS3.Core`中的`Extensions`类。通过`EntryPoint`类，主程序运行时才能识别用户自定义的数字化对象实体类。

## 1. Extensions类说明

`iS3.Core`中的`Extensions`类是用户实现自定义扩展类所依赖的重要对象。所有用户自定义的扩展类均需继承自该`Extensions`类，否则无法实现扩展。`Extensions`类的定义如下所示：

```csharp
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

自定义的扩展类继承自`Extensions`类后，编译生成的动态链接文件应该放置在`\bin\extensions`目录下，否则无法被主程序识别。`iS3.Core`中的`ExtensionsManager`类将读取放置于`\bin\extensions`目录下的`dll`文件，从类名中创建出实体。



## 2. EntryPoint类的实现

此处，我们将演示如何实现扩展类的入口——`EntryPoint`类，并对其函数进行简要介绍。

以同济大学项目的演示为例，代码如下：

```csharp
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

此处，我们首先需要继承`Extensions`类，以声明自定义类的扩展属性。其次，我们还需重载`Extensions`类的三个函数，尤其是 `name()` 函数，这是识别出自定义类所处的命名空间的关键。

**重载函数说明**

- `name()`：返回所处的命名空间。此处所处的`namespace`为`iS3.Geology`，所以应该返回"iS3.Geology"。
- `provider()` ：返回开发团队名称。
- `version()`：返回版本信息。

