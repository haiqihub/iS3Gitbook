# 实现`EntryPoint`类

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

