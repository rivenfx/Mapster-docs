### 映射到一个新的对象
Mapster 创建 **目标对象** 并将值映射到它。

```csharp
var destObject = sourceObject.Adapt<Destination>();
```

### 映射到现有对象

创建一个对象，Mapster将把 **源对象** 映射到这个对象。

```csharp
sourceObject.Adapt(destObject);
```

### Queryable Extensions

Mapster 还提供了对 IQueryable 的映射扩展。

```csharp
using (MyDbContext context = new MyDbContext())
{
    // 使用 ProjectToType 映射到目标类型
    var destinations = context.Sources.ProjectToType<Destination>().ToList();

    // 手动编写映射
    var destinations = context.Sources.Select(c => new Destination {
        Id = p.Id,
        Name = p.Name,
        Surname = p.Surname,
        ....
    })
    .ToList();
}
```