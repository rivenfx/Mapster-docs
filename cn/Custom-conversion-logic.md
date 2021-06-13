# 自定义映射逻辑

### 自定义类型映射

使用 `MapWith` 方法可以指定特定类型的映射逻辑。

例如将字符串映射为 `char[]`:

```csharp
//Example of transforming string to char[].
TypeAdapterConfig<string, char[]>.NewConfig()
    .MapWith(str => str.ToCharArray());
```

在某些情况下，应该将映射行为修改为复制实例而不是深度拷贝，出现这种情况时同样可以使用 `MapWith`。

例如，配置 `JObject` 映射到 `JObject` ：

 ```csharp
TypeAdapterConfig<JObject, JObject>.NewConfig()
    .MapWith(json => json);
 ```

`MapWith` 默认不和其它映射配置产生关系，如果想要应用其它映射配置，例如 `PreserveReference`  、  `Include`  `AfterMapping` 等方法，只需在调用 `MapWith` 方法的时候将 `applySettings` 参数设置为 `true` 即可：

```csharp
TypeAdapterConfig<ComplexPoco, ComplexDto>.NewConfig()
    .PreserveReference(true)
    .MapWith(poco => poco.ToDto(), applySettings: true);
```

### 自定义到现有对象的映射数据

通过 `MapToTargetWith` 方法可以控制到现有对象逻辑的映射。

例如，可以将数据复制到现有数组中：

```csharp
TypeAdapterConfig<string[], string[]>.NewConfig()
    .MapToTargetWith((src, dest) => Array.Copy(src, dest, src.Length));
```

> 注意！如果配置了 `MapWith`，但没有配置 `MapToTargetWith`，Mapster 将使用 `MapWith` 的映射配置。

### 自定义映射完成后的操作

有些情况下，不需要完全自定义映射逻辑；可以让 Mapster 自动处理映射，然后通过 `AfterMapping` 方法在映射完成后对映射结果进行处理：

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
    .AfterMapping((src, dest) => SpecialSetFn(src, dest));
```