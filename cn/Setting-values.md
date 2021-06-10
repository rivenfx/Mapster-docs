# 设置值

### 计算值

可以使用 `Map` 方法指定计算值的逻辑。例如，根据姓 和名 字段生成完整的姓名:

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
                            .Map(dest => dest.FullName, src => src.FirstName + " " + src.LastName);
```

### 变换值

While `Map` method specify logic for single property, `AddDestinationTransform` allows transforms for all items of a type, such as trimming all strings. But really any operation can be performed on the destination value before assignment.

当 `Map`方法为单个属性指定逻辑时，' AddDestinationTransform '允许对类型的所有项进行转换，例如修剪所有字符串。但实际上，在赋值之前可以对目标值执行任何操作。

**Trim string**

```csharp
TypeAdapterConfig<TSource, TDestination>.NewConfig()
        .AddDestinationTransform((string x) => x.Trim());
```

**Null replacement**
```csharp
TypeAdapterConfig<TSource, TDestination>.NewConfig()
        .AddDestinationTransform((string x) => x ?? "");
```

**Return empty collection if null**
```csharp
config.Default.AddDestinationTransform(DestinationTransform.EmptyCollectionIfNull);
```

### Passing run-time value

In some cases, you might would like to pass runtime values (ie, current user). On configuration, we can receive run-time value by `MapContext.Current.Parameters`.

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
                            .Map(dest => dest.CreatedBy,
                                 src => MapContext.Current.Parameters["user"]);
```

To pass run-time value, we need to use `BuildAdapter` method, and call `AddParameters` method to add each parameter.

```csharp
var dto = poco.BuildAdapter()
              .AddParameters("user", this.User.Identity.Name)
              .AdaptToType<Dto>();
```