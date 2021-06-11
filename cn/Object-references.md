# Object references

### Preserve reference (preventing circular reference stackoverflow)

When mapping objects with circular references, a stackoverflow exception will result. This is because Mapster will get stuck in a loop trying to recursively map the circular reference. If you would like to map circular references or preserve references (such as 2 properties pointing to the same object), you can do it by setting `PreserveReference` to `true`

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .PreserveReference(true);
```

NOTE: in Mapster setting is per type pair, not per hierarchy (see https://github.com/MapsterMapper/Mapster/wiki/Config-for-nested-classes). Therefore, you need to apply config to all type pairs.

NOTE: you might need to use `MaxDepth`. `PreserveReference` doesn't support EF Query (`ProjectTo`)

### MaxDepth

Rather than `PreserveReference`, you could also try `MaxDepth`. `MaxDepth` will map until it reaches the defined limit. Unlike `PreserveReference`, `MaxDepth` also works with queryable projection.

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .MaxDepth(3);
```

NOTE 1: `MaxDepth` starts with 1, means you will copy only primitives. POCO class and collection of POCO each count as a depth of 1.

NOTE 2: even `MaxDepth` has no maximum value, you shouldn't input large number. Each depth will generate a mapping logic, otherwise it will consume a lot of memory.

### Shallow copy

By default, Mapster will recursively map nested objects. You can do shallow copying by setting `ShallowCopyForSameType` to `true`.

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .ShallowCopyForSameType(true);
```

### Mapping very large objects

For performance optimization, Mapster tried to inline class mapping. This process will takes time if your models are complex.

![image-20210611093257011](Object-references.assets/image-20210611093257011.png)

You can skip inlining process by:

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .AvoidInlineMapping(true);
```