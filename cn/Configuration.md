# 映射配置



### 映射配置

使用 `TypeAdapterConfig<TSource, TDestination>.NewConfig()`  或  `TypeAdapterConfig<TSource, TDestination>.ForType()` 配置类型映射；

当调用 `NewConfig` 方法时，将会覆盖已存在的类型映射配置。

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Ignore(dest => dest.Age)
    .Map(dest => dest.FullName,
        src => string.Format("{0} {1}", src.FirstName, src.LastName));
```



如果你不想覆盖之前已经创建好的映射配置，那你可以使用  `TypeAdapterConfig<TSource, TDestination>.ForType()`；

`ForType` 方法与 `NewConfig` 的差别：如果指定类型映射配置不存在，那它将创建一个新的映射，如果指定类型的映射配置已存在，那么它将会扩展已有的映射配置，而不是删除或替换已有的映射配置。

```csharp
TypeAdapterConfig<TSource, TDestination>
        .ForType()
        .Ignore(dest => dest.Age)
        .Map(dest => dest.FullName,
            src => string.Format("{0} {1}", src.FirstName, src.LastName));
```



### 全局设置

使用全局设置将映射策略应用到所有的映射配置。

```csharp
TypeAdapterConfig.GlobalSettings.Default.PreserveReference(true);
```



对于特定的类型映射，你可以使用 `TypeAdapterConfig<SimplePoco, SimpleDto>.NewConfig()` 覆盖全局映射配置。

```csharp
TypeAdapterConfig<SimplePoco, SimpleDto>.NewConfig().PreserveReference(false);
```



### 基于规则的映射配置

你可以使用 `When` 方法，当满足某个条件时，进行一些特定的映射操作。

下面的这个例子，当任何一个映射的 源类型和目标类型 相同时，不映射 `Id` 属性：

```csharp
TypeAdapterConfig.GlobalSettings.When((srcType, destType, mapType) => srcType == destType)
    .Ignore("Id");
```



在下面这个例子中，映射配置只对 `IQueryable` 生效：

```csharp
TypeAdapterConfig.GlobalSettings.When((srcType, destType, mapType) => mapType == MapType.Projection)
    .IgnoreAttribute(typeof(NotMapAttribute));
```



### 只配置目标类型

在不确定源类型的时候，使用 `ForDestinationType ` 来创建针对于 目标类型 的映射配置。

比如使用 `AfterMapping ` 在映射完成后调用目标类型对象的 `Validate` 方法：

```csharp
TypeAdapterConfig.GlobalSettings.ForDestinationType<IValidator>()
                    .AfterMapping(dest => dest.Validate());
```

> 注意！在上面的代码段中指定目标类型为 `IValidator` 接口，那么将会把映射配置应用到所有实现了 `IValidator` 的类型。



### 泛型类型映射

如果映射的是泛型类型，可以通过将泛型类型传给 `ForType` 来创建设置.

```csharp
TypeAdapterConfig.GlobalSettings.ForType(typeof(GenericPoco<>), typeof(GenericDto<>))
    .Map("value", "Value");
```