# EF 6 & EF Core

### EF 6 & EF Core 支持

在EF中，对象是被追踪的，当从 Dto 复制数据到包含导航属性的实体时，这个插件将自动在导航属性中找到实体对象。

**EF 6***

    PM> Install-Package Mapster.EF6

**EF Core**

    PM> Install-Package Mapster.EFCore



#### Mapster.EFCore & EFCore 兼容性
- EFCore 5.x  —— Mapster.EFCore 5.x
- EFCore 3.x  —— Mapster.EFCore 3.x
- EFCore 1.x  —— Mapster.EFCore 2.x


#### 如何使用

使用 `EntityFromContext` 方法来设置使用的 `DbContext` 对象:

```csharp
var poco = myDbContext.DomainPoco.Include("Children")
    .Where(item => item.Id == dto.Id).FirstOrDefault();

dto.BuildAdapter()
    .EntityFromContext(myDbContext)
    .AdaptTo(poco);
```

如果使用 `IMapper` 实例：

```csharp
_mapper.From(dto)
    .EntityFromContext(myDbContext)
    .AdaptTo(poco);
```

#### IMapper 实例对 EF Core ProjectToType 的支持
通过 `Mapster.EFCore` 插件的支持， `IMapper` 实例也可以使用 `ProjectToType` 方法对 EF Core 的查询进行映射：

```csharp
var query = myDbContext.Customers.Where(...);
_mapper.From(query)
    .ProjectToType<Dto>();
```