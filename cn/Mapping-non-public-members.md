# 映射非公开的成员

### EnableNonPublicMembers

This will allow Mapster to set to all non-public members.

```csharp
//type pair
TypeAdapterConfig<Poco, Dto>.NewConfig().EnableNonPublicMembers(true);

//global
TypeAdapterConfig.GlobalSettings.Default.EnableNonPublicMembers(true);
```

### AdaptMember attribute

You can also map non-public members with `AdaptMember` attribute.

```csharp
public class Product 
{
    [AdaptMember]
    private string HiddenId { get; set; }
    public string Name { get; set; }
}
```

### Map

`Map` command can map to private member by specify name of the members.

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Map("PrivateDestName", "PrivateSrcName");
```

### IncludeMember

With `IncludeMember`, you can select which access modifier to allow.

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IncludeMember((member, side) => member.AccessModifier == AccessModifier.Internal 
                                     || member.AccessModifier == AccessModifier.ProtectedInternal);
```

### Note for non-public member mapping

If type doesn't contain public properties, Mapster will treat type as primitive, you must also declare type pair to ensure Mapster will apply non-public member mapping.

```csharp
TypeAdapterConfig.GlobalSettings.Default.EnableNonPublicMembers(true);
TypeAdapterConfig<PrivatePoco, PrivateDto>.NewConfig();
```