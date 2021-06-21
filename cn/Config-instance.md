# 配置实例



### 配置实例

在 Mapster 中，默认的配置实例为 `TypeAdapterConfig.GlobalSettings` ，如果需要在不同场景下有不同的映射配置，Mapster  提供了 `TypeAdapterConfig` 用于实现此需求：

```csharp
var config = new TypeAdapterConfig();
config.Default.Ignore("Id");
```

如何给 配置实例 添加l类型映射配置？

直接使用 `NewConfig` 和 `ForType` 方法即可：

```csharp
config.NewConfig<TSource, TDestination>()
        .Map(dest => dest.FullName,
            src => string.Format("{0} {1}", src.FirstName, src.LastName));

config.ForType<TSource, TDestination>()
        .Map(dest => dest.FullName,
            src => string.Format("{0} {1}", src.FirstName, src.LastName));
```

通过将  配置实例 作为参数传给 `Adapt` 方法来应用特定的类型映射配置：

> 注意！ 配置实例 在程序中一定要作为单例存在，否则会影响性能！

```csharp
var result = src.Adapt<TDestination>(config);
```

也可以创建一个指定  `TypeAdapterConfig` 的 `Mapper`，使用 `Mapper` 来做映射：

```csharp
var mapper = new Mapper(config);
var result = mapper.Map<TDestination>(src);
```

### 复制配置实例

如果想从现有的配置中创建配置实例，可以使用 `Clone` 方法。

例如 复制全局配置实例 ：

```csharp
var newConfig = TypeAdapterConfig.GlobalSettings.Clone();
```

或者复制其它配置实例：

```csharp
var newConfig = oldConfig.Clone();
```

### Fork 配置

`Fork` 方法内部直接调用 `Clone` 方法，但是使用 `Fork` 方法的形式与使用 `Clone` 方法有些许差别。

`Fork` 方法通过传入委托方法直接增加新的映射配置：

```csharp
var forked = mainConfig.Fork(config => 
				{
					config.ForType<Poco, Dto>()
						.Map(dest => dest.code, src => src.Id);
				});

var dto = poco.Adapt<Dto>(forked);
```

以上代码等同于使用 `Clone` 方法实现的以下代码:

```c#
var forked = mainConfig.Clone();
forked.ForType<Poco, Dto>()
      .Map(dest => dest.code, src => src.Id);

var dto = poco.Adapt<Dto>(forked);
```

