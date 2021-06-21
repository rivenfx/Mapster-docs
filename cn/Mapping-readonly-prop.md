# 映射只读属性

### 映射非公开 `set` 的属性

Mapster 默认会自动映射非公开 `set` 的属性：

```csharp
public class Order {
    public string Id { get; set; }
    public ICollection<OrderItem> Items { get; private set; }
}
```

### [UseDestinationValue] 特性标签

给属性添加特性标签 `[UseDestinationValue]` ，未指定 `set` 的属性也能进行映射：

```csharp
public class Order {
    public string Id { get; set; }

    [UseDestinationValue]
    public ICollection<OrderItem> Items { get; } = new List<OrderItem>();
}
```

### 基于约定配置映射

如果希望所有未指定 `set` 的 `ICollection<>` 都参与映射，那么可以使用 `UseDestinationValue` 方法进行配置：

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .UseDestinationValue(member => member.SetterModifier == AccessModifier.None &&
                                   member.Type.IsGenericType &&
                                   member.Type.GetGenericTypeDefinition() == typeof(ICollection<>));
```