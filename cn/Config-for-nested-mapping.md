# 配置嵌套映射

例如有以下 父类、子类：

```csharp
public class ParentPoco
{
    public string Id { get; set; }
    public List<ChildPoco> Children { get; set; }
    public string Name { get; set; }
}
public class ChildPoco
{
    public string Id { get; set; }
    public List<GrandChildPoco> GrandChildren { get; set; }
}
public class GrandChildPoco
{
    public string Id { get; set; }
}
```

如果你配置了父类型：

```csharp
TypeAdapterConfig<ParentPoco, ParentDto>.NewConfig()
    .PreserveReference(true);
```

默认情况下，子类型不会从 `PreserveReference` 中得到效果。

因此必须在 `ParentPoco` 中指定所有类型映射:

```csharp
TypeAdapterConfig<ParentPoco, ParentDto>.NewConfig()
    .PreserveReference(true);
TypeAdapterConfig<ChildPoco, ChildDto>.NewConfig()
    .PreserveReference(true);
TypeAdapterConfig<GrandChildPoco, GrandChildDto>.NewConfig()
    .PreserveReference(true);
```

或者可以调用 全局配置实例 的 `PreserveReference` 方法：

```csharp
TypeAdapterConfig.GlobalSettings.Default.PreserveReference(true);
```

### Fork

你可以使用 `Fork` 方法来定义仅将指定的映射应用于嵌套映射而不污染全局设置的配置：

```csharp
TypeAdapterConfig<ParentPoco, ParentDto>.NewConfig()
    .Fork(config => config.Default.PreserveReference(true));
```

**忽略为null或为空的字符串**

再比如，Mapster 只能忽略 null 值 ([IgnoreNullValues](Shallow-merge.md#copy-vs-merge))，但是你可以使用 `Fork` 来忽略 null 或空值。 

```csharp
TypeAdapterConfig<ParentPoco, ParentDto>.NewConfig()
    .Fork(config => config.ForType<string, string>()
        .MapToTargetWith((src, dest) => string.IsNullOrEmpty(src) ? dest : src)
    );
```