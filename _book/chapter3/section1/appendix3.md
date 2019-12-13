# 主键

​	Entity Framework Code First的默认主键约束：*属性名为[ID]或[类名 + ID]*。如在Product类中，Entity Framework Code First会根据默认约定将类中名称为ID或ProductID的属性设置为主键。Entity Framework Code First主键的默认约定也一样可以进行重写，重新根据需要进行设置。

## Data Annotation 方式

```csharp
    using System.ComponentModel.DataAnnotations;
    using System.ComponentModel.DataAnnotations.Schema;
```

```csharp
    [Key]
    [Column("ProductID")]
    public int ProductID { get; set; }
```



## Fluent API方式

```csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>().HasKey(t => t.ProductID);
    }
```



 若一个表有多个主键时：

```csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>().HasKey(t => new { t.KeyID, t.CandidateID });
    }[Column("UnitPrice", TypeName = "MONEY")]
public decimal UnitPrice { get; set; }
```

