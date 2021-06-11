# Naming convention

### Flexible name

By default, Mapster will map property with case sensitive name. You can adjust to flexible name mapping by setting `NameMatchingStrategy.Flexible` to `NameMatchingStrategy` method. This setting will allow matching between `PascalCase`, `camelCase`, `lower_case`, and `UPPER_CASE`. 

```csharp
//global
TypeAdapterConfig.GlobalSettings.Default.NameMatchingStrategy(NameMatchingStrategy.Flexible);

//type pair
TypeAdapterConfig<Foo, Bar>.NewConfig().NameMatchingStrategy(NameMatchingStrategy.Flexible);
```

### Ignore cases

Flexible name could not map between `MiXcAsE` and `MixCase`, because flexible name will break `MiXcAsE` into `Mi-Xc-As-E` rather than just `Mix-Case`. In this case, we need to use `IgnoreCase` to perform case insensitive matching.

```csharp
TypeAdapterConfig.GlobalSettings.Default.NameMatchingStrategy(NameMatchingStrategy.IgnoreCase);
```

### Prefix & Replace

For custom rules, you can use either `ConvertSourceMemberName` or `ConvertDestinationMemberName` up to which side you would like to convert. For example, you might would like to add `m_` to all properties.

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
    .NameMatchingStrategy(NameMatchingStrategy.ConvertSourceMemberName(name => "m_" + name));
```

This example is to replace foreign letter from name.

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
    .NameMatchingStrategy(NameMatchingStrategy.ConvertSourceMemberName(name => name.Replace("Ä", "A"));
```

### Naming Convention with IDictionary<string, T>

If you would like to change case from POCO to `IDictionary<string, T>` to camelCase, you can use `ToCamelCase`. Another way around, if you would like to map `IDictionary<string, T>` back to POCO, you can use `FromCamelCase`.

```csharp
TypeAdapterConfig<Poco, Dictionary<string, object>>.NewConfig()
    .NameMatchingStrategy(NameMatchingStrategy.ToCamelCase);
TypeAdapterConfig<Dictionary<string, object>, Poco>.NewConfig()
    .NameMatchingStrategy(NameMatchingStrategy.FromCamelCase);
```

NOTE: mapping from `IDictionary<string, T>` to POCO, you can also use `Flexible` or `IgnoreCase`, but both will be slower since it will scan through dictionary entries rather than lookup.

### Rule based Naming

You can change name based on rule by `GetMemberName` method. For example, if we would like to rename property based on `JsonProperty` attribute.

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .GetMemberName(member => member.GetCustomAttributes(true)
                                    .OfType<JsonPropertyAttribute>()
                                    .FirstOrDefault()?.PropertyName);  //if return null, property will not be renamed
```

Then in your class

```csharp
public class Poco 
{
    [JsonProperty("code")]
    public string Id { get; set; }

    ...
}
```

With above config, `Id` will be mapped to `code`.