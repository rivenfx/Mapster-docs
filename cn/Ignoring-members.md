# 映射忽略成员

### 基本的映射忽略

Mapster 默认会自动映射所有符合规则的成员。如果想要在映射时忽略某一个成员，可以使用 `Ignore` 方法进行配置：

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Ignore(dest => dest.Id);
```

### 基于规则的映射忽略

使用 `IgnoreMember` 方法可以根据某个条件决定是否映射成员：

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IgnoreMember((member, side) => !validTypes.Contains(member.Type));
```

> 更多信息请参阅 [基于规则映射成员](Rule-based-member-mapping.md)



### 忽略未显示映射的成员

使用 `IgnoreNonMapped` 方法可以忽略所有未显示配置映射的成员。

例如，只映射 `Id` 和 `Name`：

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Map(dest => dest.Id, src => src.Id)
    .Map(dest => dest.Name, src => src.Name)
    .IgnoreNonMapped(true);
```



### 通过特性标签配置映射忽略

当一个成员有 `[AdaptIgnore]` 特性标记时，这个成员将不会被映射：

```csharp
public class Product {
    public string Id { get; set; }
    public string Name { get; set; }

    [AdaptIgnore]
    public decimal Price { get; set; }
}
```

> 更多信息请参阅 [使用特性标签配置映射](Setting-by-attributes.md)



### 不映射满足条件的成员

使用 `IgnoreIf` 方法，当满足条件时将忽略此成员的映射：

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .IgnoreIf((src, dest) => !string.IsNullOrEmpty(dest.Name), dest => dest.Name);
```



### 不映射空值成员

Mapster 默认映射时会将 源对象的所有成员映射到目标对象，如果不想映射空值，那么可以使用 `IgnoreNullValues` 方法进行配置：

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .IgnoreNullValues(true);
```