# Mapping readonly prop

### Non public setter

Mapster can map to non public setter automatically.

```csharp
public class Order {
    public string Id { get; set; }
    public ICollection<OrderItem> Items { get; private set; }
}
```

### Using UseDestinationValue attribute

You can make your type pure readonly and annotate with [UseDestinationValue].

```csharp
public class Order {
    public string Id { get; set; }

    [UseDestinationValue]
    public ICollection<OrderItem> Items { get; } = new List<OrderItem>();
}
```

### Convention based setup

Or you can apply without annotate each type, for example, if you would like all readonly `ICollection<>` to use destination value.

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .UseDestinationValue(member => member.SetterModifier == AccessModifier.None &&
                                   member.Type.IsGenericType &&
                                   member.Type.GetGenericTypeDefinition() == typeof(ICollection<>));
```