# Shallow merge

### Deep copy vs shallow copy

By default, Mapster will recursively map nested objects (deep copy). You can do shallow copying by setting `ShallowCopyForSameType` to `true`.

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .ShallowCopyForSameType(true);
```
### Copy vs Merge

By default, Mapster will map all properties, even source properties containing null values. You can copy only properties that have values (merge) by using `IgnoreNullValues` method.

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .IgnoreNullValues(true);
```