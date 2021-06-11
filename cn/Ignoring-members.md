# Ignoring members

### Ignore

Mapster will automatically map properties with the same names. You can ignore members by using the `Ignore` method.

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Ignore(dest => dest.Id);
```

### Rule based ignore

You can ignore based on member information by `IgnoreMember` command. Please see https://github.com/MapsterMapper/Mapster/wiki/Rule-based-member-mapping for more info.

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IgnoreMember((member, side) => !validTypes.Contains(member.Type));
```

### IgnoreNonMapped

You can ignore all non-mapped members by IgnoreNonMapped command. For example, we would like to map only Id and Name.

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Map(dest => dest.Id, src => src.Id)
    .Map(dest => dest.Name, src => src.Name)
    .IgnoreNonMapped(true);
```

### Ignore by attribute

You can ignore member by decorate with `[AdaptIgnore]`, and you can ignore custom attributes by `IgnoreAttribute` command. Please see https://github.com/MapsterMapper/Mapster/wiki/Setting-by-attributes for more info.

```csharp
public class Product {
    public string Id { get; set; }
    public string Name { get; set; }

    [AdaptIgnore]
    public decimal Price { get; set; }
}
```

### Ignore conditionally

You can ignore members conditionally, with condition based on source or target. When the condition is met, mapping of the property will be skipped altogether. This is the difference from custom `Map` with condition, where destination is set to `null` when condition is met.

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .IgnoreIf((src, dest) => !string.IsNullOrEmpty(dest.Name), dest => dest.Name);
```

### IgnoreNullValues

You might would like to merge from input object, By default, Mapster will map all properties, even source properties containing null values. You can copy only properties that have values by using `IgnoreNullValues` method.

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .IgnoreNullValues(true);
```