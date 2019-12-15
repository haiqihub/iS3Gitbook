# 数字类型长度及精度

 在Product类中，UnitPrice表示单价，对于价格类的字段，我们通常会希望其保留2为小数。这时可以使用Fluent API进行设置，且Data Annotation不支持该设置。

```csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>().Property(t => t.UnitPrice)
            .HasColumnName("UnitPrice")
            .HasPrecision(18, 2);
    }
```
