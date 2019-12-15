# Fluent API配置Configuration映射类

​	在使用Fluent API进行Entity Framework Code First数据库映射时，除了以上的在重写OnModelCreating方法中直接对Entity进行配置之外，也可以对Configurations进行配置。这时可以先写一个单独的类，将数据表的全部映射要求都写在构造函数中。

#### ProductMap.cs类

```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.ComponentModel.DataAnnotations.Schema;
    using System.Data.Entity.ModelConfiguration;
    using Portal.Entities;

    namespace Portal.Mapping
    {
        public class ProductMap : EntityTypeConfiguration<Product>
        {
            public ProductMap()
            {
                // Primary Key
                this.HasKey(t => t.ProductID);

                // Properties
                this.Property(t => t.ProductID)
                    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
                this.Property(t => t.ProductName)
                    .IsRequired()
                    .HasMaxLength(100);

                // Table & Column Mappings
                this.ToTable("Product");
                this.Property(t => t.ProductID).HasColumnName("ProductID");
                this.Property(t => t.ProductName).HasColumnName("ProductName");
                this.Property(t => t.UnitPrice)
                    .HasColumnName("UnitPrice")
                    .HasPrecision(18, 2);
            }
        }
    }
```

有了上面的映射类之后，在转换OnModelCreating方法中则可以直接调用映射类，从而减少了OnModelCreating方法的复杂度，同时也增强了代码维护的替代性。

```csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Configurations.Add(new ProductMap());
    }
```

