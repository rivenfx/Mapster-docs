# Rule based member mapping

By default, Mapster will include public fields and properties, but we can change this behavior by `IncludeMember` and `IgnoreMember` method. The methods require predicate, and input types of predicate are:

```csharp
public interface IMemberModel
{
    Type Type { get; }
    string Name { get; }
    object Info { get; }
    AccessModifier SetterModifier { get; }
    AccessModifier AccessModifier { get; }
    IEnumerable<object> GetCustomAttributes(bool inherit);
}

public enum MemberSide
{
    Source,
    Destination,
}
```

### Not allow fields

If you would like to allow only properties not public field to be mapped, you can check from `Info`. Possible values could be `PropertyInfo`, `FieldInfo`, or `ParameterInfo`. In this case, we will reject member of type `FieldInfo`.

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IgnoreMember((member, side) => member.Info is FieldInfo);
```

### Allow only some list of types to be mapped

Suppose you are working with EF, and you would like to skip all navigation properties. Then we will allow only short list of types.

**Allow by types**
```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IgnoreMember((member, side) => !validTypes.Contains(member.Type));
```
**Allow by Namespaces**
```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IgnoreMember((member, side) => !member.Type.Namespace.StartsWith("System"));
```

### Allow internal members

If you would like to map members marked as internal, you can do it by:

```csharp
    TypeAdapterConfig.GlobalSettings.Default
        .IncludeMember((member, side) => member.AccessModifier == AccessModifier.Internal 
                                         || member.AccessModifier == AccessModifier.ProtectedInternal);
```

### Allow only DataMember attribute

If you would like to include all members decorated with DataMember attribute, and ignore all members with no DataMember attribute, you can set up by:

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IncludeMember((member, side) => member.GetCustomAttributes(true).OfType<DataMemberAttribute>().Any());
TypeAdapterConfig.GlobalSettings.Default
    .IgnoreMember((member, side) => !member.GetCustomAttributes(true).OfType<DataMemberAttribute>().Any());
```

### Turn-off non-public setters

Mapster always allows non-public setters. But you can override by:

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IgnoreMember((member, side) => side == MemberSide.Destination
                                    && member.SetterModifier != AccessModifier.Public);
```