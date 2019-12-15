# 字段名、长度、数据类型及是否可空

​	在默认约定的情况下，Entity Framework Code First创建的列名与类的属性名相同，可以根据需要进行重新指定类属性与列名之间的映射关系。



## Data Annotation 方式

```csharp
[Column("ProductID")]
public int ProductID { get; set; }
[MaxLength(100)]
[Required, Column("ProductName")]
public string ProductName { get; set; }
```

在使用Required特性(Attribute)设置字段不允许为空时，需要添加命名空间引用：

```csharp
using System.ComponentModel.DataAnnotations;
```



## Fluent API方式

```csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Entity<Product>().Property(t => t.ProductID)
        .HasColumnName("ProductID");
    modelBuilder.Entity<Product>().Property(t => t.ProductName)
        .IsRequired()
        .HasColumnName("ProductName")
        .HasMaxLength(100);
}
```



 在默认情况下，`int`类型的属性生成的列名对应SQL SERVER列`int`类型；而`String`类型的属性则对应`SQL SERVER`列的`NVARCHAR`类型。若类的字符串类型属性未设置`MaxLength`，则生成对应的列类型为`NVARCHAR(MAX)`。

 为属性指定对应的`SQL SERVER`数据类型：

```csharp
[Column("UnitPrice", TypeName = "MONEY")]
public decimal UnitPrice { get; set; }
```

重写`OnModelCreating`方法，配置类对应于数据库中的表名：

```csharp
 protected override void OnModelCreating(DbModelBuilder  modelBuilder)
 {
     modelBuilder.Entity<Product>().ToTable("Product");
 }
```

```csharp
modelBuilder.Entity<Product>().Property(t => t.UnitPrice)
        .HasColumnName("UnitPrice")
        .HasColumnType("MONEY");
```

 到此步，`Product.cs`类文件的完整代码如下：

```csharp
 	using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;

    using System.ComponentModel.DataAnnotations;
    using System.ComponentModel.DataAnnotations.Schema;

    namespace Portal.Entities
    {
        [Table("Product", Schema = "dbo")]
        public class Product
        {
            [Column("ProductID")]
            public int ProductID { get; set; }

            [MaxLength(100)]
            [Required, Column("ProductName")]
            public string ProductName { get; set; }

            [Column("UnitPrice", TypeName = "MONEY")]
            public decimal UnitPrice { get; set; }
        }
    }
```

属性设置text数据类型：

```csharp
[Column("Remark", TypeName = "text")]
public string Remark { get; set; }
```

```csharp
modelBuilder.Entity<Category>().Property(t => t.Remark)
        .HasColumnName("Remark")
        .HasColumnType("text");
```

