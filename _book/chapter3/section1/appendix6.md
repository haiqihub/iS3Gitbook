# 非数据库字段属性

在类中，如果有一些属性不需要映射到对应生成的数据表中，可以通过以下方式设置。



## Data Annotation 方式

```csharp
    [NotMapped]
    public string Remark { get; set; }
```



## Fluent API 方式

```csharp
     protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>().Ignore(t => t.Remark);
    }
```
