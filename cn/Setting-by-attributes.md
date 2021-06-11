# Setting by attributes



### AdaptIgnore attribute

When a property decorated with `[AdaptIgnore]`, that property will be excluded from Mapping. For example, if we would like to exclude price to be mapped.

```csharp
public class Product {
    public string Id { get; set; }
    public string Name { get; set; }

    [AdaptIgnore]
    public decimal Price { get; set; }
}
```

`[AdaptIgnore]` will both ignore when type are used as source or destination. You can ignore only one side by passing `MemberSide`.

```csharp
public class Product {
    public string Id { get; set; }
    public string Name { get; set; }

    [AdaptIgnore(MemberSide.Source)]
    public decimal Price { get; set; }
}
```

Above example, `Price` will be ignored only when `Product` is used as source.

### Ignore custom attributes

You can ignore members annotated with any attributes by using the `IgnoreAttribute` method.

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IgnoreAttribute(typeof(JsonIgnoreAttribute));
```

However `IgnoreAttribute` will ignore both source and destination. If you would like to ignore only one side, you can use `IgnoreMember`.

```csharp
config.IgnoreMember((member, side) => member.HasCustomAttribute(typeof(NotMapAttribute)) && side == MemberSide.Source);
```

### AdaptMember attribute
**Map to different name**  
With `AdaptMember` attribute, you can specify name of source or target to be mapped. For example, if we would like to map `Id` to `Code`.

```csharp
public class Product {
    [AdaptMember("Code")]
    public string Id { get; set; }
    public string Name { get; set; }
}
```

**Map to non-public members**  
You can also map non-public members with `AdaptMember` attribute.
```csharp
public class Product {
    [AdaptMember]
    private string HiddenId { get; set; }
    public string Name { get; set; }
}
```

### Rename from custom attributes

You can rename member to be matched by `GetMemberName`. For example, if we would like to rename property based on `JsonProperty` attribute.

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .GetMemberName(member => member.GetCustomAttributes(true)
                                    .OfType<JsonPropertyAttribute>()
                                    .FirstOrDefault()?.PropertyName);  //if return null, property will not be renamed
```

### Include custom attributes

And if we would like to include non-public members decorated with `JsonProperty` attribute, we can do it by `IncludeAttribute`.

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IncludeAttribute(typeof(JsonPropertyAttribute));
```

### Use destination value

You can tell Mapster to use existing property object to map data rather than create new object.

```csharp
public class Order {
    public string Id { get; set; }

    [UseDestinationValue]
    public ICollection<OrderItem> Items { get; } = new List<OrderItem>();
}
```