# 基本用法

### 映射到一个新的对象

Mapster 将会创建一个目标类型的对象，并将值设置到目标对象。

```csharp
var destObject = sourceObject.Adapt<Destination>();
```

### 映射到现有对象

创建一个对象，Mapster 将把 **源对象** 映射到这个对象。

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