# 表名及所有者

​	在先前约定的情况下，Entity Framework Code First创建的表名是根据类名的英语复数形式，创建的表所有者为`dbo`，可以通过替换约定来指定表名及表的所有者。

## Data Annotation 方式

在使用Data Annotation方式进行Entity Framework Code First与数据库映射之前，需要先添加命名空间引用。

```csharp
using System.ComponentModel.DataAnnotations.Schema;
```

为类配置对应表名：

```csharp
[Table("Product")]
 public class Product
```

为类配置对应表名并指定表的所有者：

```csharp
 [Table("Product", Schema = "dbo")]
 public class Product
```



## Fluent API方式

Fluent API实现配置Entity Framework Code First与数据库映射关系主要是通过继承DbContext并重写其中的OnModelCreating方法来进行的。在本文中新建类文件PortalContext.cs继承DbContext。



在继承`DbContext`之前，添加命名空间引用。

```csharp
 using System.Data.Entity;
```

重写`OnModelCreating`方法，配置类对应于数据库中的表名：

```csharp
 protected override void OnModelCreating(DbModelBuilder  modelBuilder)
 {
     modelBuilder.Entity<Product>().ToTable("Product");
 }
```

重写`OnModelCreating`方法，配置类对应于数据库中的表名，并指定表的所有者：

```csharp
 protected override void OnModelCreating(DbModelBuilder  modelBuilder)
 {
     modelBuilder.Entity<Product>().ToTable("Product", "dbo");
 }
```

 到此处`PortalContext.cs`的完整代码：

```csharp
 using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Data.Entity;
    using Portal.Entities;

    namespace Portal
    {
        public class PortalContext : DbContext
        {
            static PortalContext()
            {
                Database.SetInitializer(new DropCreateDatabaseIfModelChanges<PortalContext>());
            }

            public PortalContext()
                : base("name=PortalContext")
            {
            }

            public DbSet<Product> Products { get; set; }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                modelBuilder.Entity<Product>().ToTable("Product", "dbo");
            }
        }
    }
```