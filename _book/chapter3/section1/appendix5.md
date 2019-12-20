# 数字类型长度及精度

在`Product`类中，`UnitPrice`表示单价，对于价格类的字段，我们通常会希望其保留2为小数。这时可以使用`Fluent API`进行设置，且`Data Annotation`不支持该设置。

```csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>().Property(t => t.UnitPrice)
            .HasColumnName("UnitPrice")
            .HasPrecision(18, 2);
    }
```

