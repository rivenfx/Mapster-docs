# 映射非公开的成员

### 启用映射非公开成员

调用 `EnableNonPublicMembers` 方法配置启用映射非公开成员：

```csharp
// 特定类型
TypeAdapterConfig<Poco, Dto>.NewConfig().EnableNonPublicMembers(true);

// 全局默认
TypeAdapterConfig.GlobalSettings.Default.EnableNonPublicMembers(true);
```

### [AdaptMember ] 特性标签

通过给非公开成员添加 `[AdaptMember]` 特性标签实现映射非公开成员：

```csharp
public class Product 
{
    [AdaptMember]
    private string HiddenId { get; set; }
    public string Name { get; set; }
}
```

### 使用 Map 方法

使用 `Map` 方法可以通过指定成员名称映射非公开成员：

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Map("PrivateDestName", "PrivateSrcName");
```

### 使用 IncludeMember 方法

使用 `IncludeMember` 方法，可以选择根据成员的访问修饰符决定是否进行映射：

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IncludeMember((member, side) => member.AccessModifier == AccessModifier.Internal 
                                     || member.AccessModifier == AccessModifier.ProtectedInternal);
```

### 映射非公开成员的注意事项

如果类型中不存在任何公开成员，Mapster 将不会映射非公开成员，若要实现非公开成员映射，那么需要显示的配置映射：

```csharp
TypeAdapterConfig.GlobalSettings.Default.EnableNonPublicMembers(true);
TypeAdapterConfig<PrivatePoco, PrivateDto>.NewConfig();
```

