### 配置实例

在 Mapster 中，默认的配置实例为 `TypeAdapterConfig.GlobalSettings` ，如果需要在不通场景下有不通的设置，Mapster  提供了 `TypeAdapterConfig` 用于实现此需求：

```csharp
var config = new TypeAdapterConfig();
config.Default.Ignore("Id");
```

如何给 `TypeAdapterConfig` 实例 添加l类型映射配置？

直接使用使用 `NewConfig` 和 `ForType` 方法即可：

```csharp
config.NewConfig<TSource, TDestination>()
        .Map(dest => dest.FullName,
            src => string.Format("{0} {1}", src.FirstName, src.LastName));

config.ForType<TSource, TDestination>()
        .Map(dest => dest.FullName,
            src => string.Format("{0} {1}", src.FirstName, src.LastName));
```

通过将  `TypeAdapterConfig` 实例 作为参数传给 `Adapt` 方法来应用特定的类型映射配置：

> 注意！不同的   `TypeAdapterConfig` 实例 在程序中一定要作为单例存在，否则会影响性能！

```csharp
var result = src.Adapt<TDestination>(config);
```

也可以创建一个指定   `TypeAdapterConfig` 的 `Mapper`，使用 `Mapper` 来做映射：

```csharp
var mapper = new Mapper(config);
var result = mapper.Map<TDestination>(src);
```

### 复制

If you would like to create configuration instance from existing configuration, you can use `Clone` method. For example, if you would like to clone from global setting.

如果想从现有的配置中创建配置实例，可以使用 `Clone` 方法。

例如 复制全局配置 ：

```csharp
var newConfig = TypeAdapterConfig.GlobalSettings.Clone();
```

或者复制其它配置实例：

```csharp
var newConfig = oldConfig.Clone();
```

### Fork

`Fork` is similar to `Clone`, but `Fork` will allow you to keep configuration and mapping in the same location. See (https://github.com/MapsterMapper/Mapster/wiki/Config-location) for more info.

```csharp
var forked = mainConfig.Fork(config => 
    config.ForType<Poco, Dto>()
            .Map(dest => dest.code, src => src.Id));

var dto = poco.Adapt<Dto>(forked);
```