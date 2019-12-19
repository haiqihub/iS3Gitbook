# 数据库自动生成字段值

`Entity Framework Code First`对于`int`类型的主键，会自动的设置其为自动增长列。但有时我们确实不需是自动增长的，可以通过以下方式进行取消自动增长。

## Data Annotation 方式

```csharp
    [Key]
    [Column("ProductID")]
    [DatabaseGenerated(DatabaseGeneratedOption.None)]
    public int ProductID { get; set; }
    [Key]
    [Column("CategoryID")]
    [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
    public int CategoryID { get; set; }
```

## Fluent API方式

```csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>().HasKey(t => t.ProductID);
        modelBuilder.Entity<Product>().Property(t => t.ProductID)
            .HasColumnName("ProductID")
            .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
    }
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>().HasKey(t => t.ProductID);
        modelBuilder.Entity<Product>().Property(t => t.ProductID)
            .HasColumnName("ProductID")
            .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
    }
```