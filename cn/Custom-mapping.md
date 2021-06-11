# Custom mapping

### Custom member mapping

You can customize how Mapster maps values to a property.

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Map(dest => dest.FullName,
        src => string.Format("{0} {1}", src.FirstName, src.LastName));
```

You can even map when source and destination property types are different.

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Map(dest => dest.Gender,      //Genders.Male or Genders.Female
        src => src.GenderString); //"Male" or "Female"
```

### Mapping with condition

The Map configuration can accept a third parameter that provides a condition based on the source.
If the condition is not met, Mapster will retry with next conditions. Default condition should be added at the end without specifying condition. If you do not specify default condition, null or default value will be assigned.

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Map(dest => dest.FullName, src => "Sig. " + src.FullName, srcCond => srcCond.Country == "Italy")
    .Map(dest => dest.FullName, src => "Sr. " + src.FullName, srcCond => srcCond.Country == "Spain")
    .Map(dest => dest.FullName, src => "Mr. " + src.FullName);
```

NOTE: if you would like to skip mapping, when condition is met, you can use `IgnoreIf` (https://github.com/MapsterMapper/Mapster/wiki/Ignoring-members#ignore-conditionally).

### Mapping to non-public members

`Map` command can map to private member by specify name of the members.

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Map("PrivateDestName", "PrivateSrcName");
```

For more information about mapping non-public members, please see https://github.com/MapsterMapper/Mapster/wiki/Mapping-non-public-members.

### Deep destination property

`Map` can be defined to map deep destination property.

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
    .Map(dest => dest.Child.Name, src => src.Name);
```

### Null propagation

If `Map` contains only property path, null propagation will be applied.

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
    .Map(dest => dest.Name, src => src.Child.Name);
```

From above example, if `src.Child` is null, mapping will return null instead of throw `NullPointerException`.

### Multiple sources

**Example 1**: Include property to Poco

```csharp
public class SubDto
{
    public string Extra { get; set; }
}
public class Dto
{
    public string Name { get; set; }
    public SubDto SubDto { get; set; }
}
public class Poco
{
    public string Name { get; set; }
    public string Extra { get; set; }
}
```

In this case, you would like to map all properties from `Dto` to `Poco`, and also include all properties from `Dto.SubDto` to `Poco`. You can do this by just mapping `dto.SubDto` to `poco` in configuration.

```csharp
TypeAdapterConfig<Dto, Poco>.NewConfig()
    .Map(poco => poco, dto => dto.SubDto);
```

**Example 2**: Mapping 2 objects to poco

In this example, you have `Dto1` and `Dto2`, and you would like to map both objects to a `Poco`. You can do this by wrapping `Dto1` and `Dto2` into a tuple. And then mapping `tuple.Item1` and `tuple.Item2` to `Poco`.

```csharp
TypeAdapterConfig<(Dto1, Dto2), Poco>.NewConfig()
    .Map(dest => dest, src => src.Item1)
    .Map(dest => dest, src => src.Item2);
```