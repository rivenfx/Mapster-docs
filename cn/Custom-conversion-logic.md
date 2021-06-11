# Custom conversion logic

### Custom type conversion

In some cases, you may want to have complete control over how an object is mapped. You can register specific transformations using the `MapWith` method.

```csharp
//Example of transforming string to char[].
TypeAdapterConfig<string, char[]>.NewConfig()
    .MapWith(str => str.ToCharArray());
```

`MapWith` also useful if you would like to copy instance rather than deep copy the object, for instance, `JObject` or `DbGeography`, these should treat as primitive types rather than POCO.

 ```csharp
TypeAdapterConfig<JObject, JObject>.NewConfig()
    .MapWith(json => json);
 ```

In case you would like to combine `MapWith` with other settings, for example, `PreserveReference`, `Include`, or `AfterMapping`, you can pass `applySettings` to true.

```csharp
TypeAdapterConfig<ComplexPoco, ComplexDto>.NewConfig()
    .PreserveReference(true)
    .MapWith(poco => poco.ToDto(), applySettings: true);
```

### Custom mapping data to existing object

You can control mapping to existing object logic by `MapToTargetWith`. For example, you can copy data to existing array.

```csharp
TypeAdapterConfig<string[], string[]>.NewConfig()
    .MapToTargetWith((src, dest) => Array.Copy(src, dest, src.Length));
```

NOTE: if you set `MapWith` setting but no `MapToTargetWith` setting, Mapster will use logic from `MapWith` setting.

### Custom actions after mapping

You might not need to specify custom mapping logic completely. You can let Mapster do the mapping, and you do logic where Mapster cannot cover by using `AfterMapping`.

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
    .AfterMapping((src, dest) => SpecialSetFn(src, dest));
```