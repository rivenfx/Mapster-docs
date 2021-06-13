# 映射值

### 计算值

可以使用 `Map` 方法指定计算值的逻辑。

例如，将 `FirstName`和`LastName` 拼接生成 `FullName`:

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
                            .Map(dest => dest.FullName, src => src.FirstName + " " + src.LastName);
```

### 变换值

`Map` 方法只能对单个成员指定映射逻辑，使用 `AddDestinationTransform` 方法可以根据类型指定映射逻辑，例如替换字符串等操作。

> 其实在 Mapster 中，只要在映射赋值操作执行之前，可以对映射的值执行任何操作。

**字符串 Trim**

```csharp
TypeAdapterConfig<TSource, TDestination>.NewConfig()
        .AddDestinationTransform((string x) => x.Trim());
```

**null字符串替换为空字符串**

```csharp
TypeAdapterConfig<TSource, TDestination>.NewConfig()
        .AddDestinationTransform((string x) => x ?? "");
```

**null集合返回空集合**

```csharp
config.Default.AddDestinationTransform(DestinationTransform.EmptyCollectionIfNull);
```

### 传递动态值

在一些情况下，可能需要传递动态传递值，可以通过调用 `MapContext.Current.Parameters` 来获取动态传递的值。

例如设置 `CreatedBy` 字段为当前登录用户名称，从 `MapContext.Current.Parameters["user"]` 映射：

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
                            .Map(dest => dest.CreatedBy,
                                 src => MapContext.Current.Parameters["user"]);
```

为了动态传递值，需要在调用映射之前使用 `BuildAdapter` 方法，并且在这之后调用 `AddParameters` 方法添加值。

下面的例子展示了如何传递动态参数 `user` ：

```csharp
var dto = poco.BuildAdapter()
              .AddParameters("user", this.User.Identity.Name)
              .AdaptToType<Dto>();
```