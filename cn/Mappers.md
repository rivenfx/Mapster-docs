### 扩展方法

你可以从任何地方调用 `Adapt` 方法。

```csharp
var dest = src.Adapt<TSource, TDestination>();
```

或者直接

```csharp
var dest = src.Adapt<TDestination>();
```

这两个扩展方法做的都是同样的事情。`src.Adapt<TDestination>` 将把 `src` 转换为 object 类型。因此，如果你转行的是值类型，那么请使用 `src.Adapt<TSource, TDestination>` 以避免不必要的装箱/拆箱。

### 映射器实例 ( Mapper )

在一些情况下，需要将 映射器 或 工厂函数 传递到依赖注入容器中。Mapster 提供了 `IMapper` 和 `Mapper` 来满足这个需求：

```c#
IMapper mapper = new Mapper();
```

并且使用 `Map` 函数来执行映射：

```c#
var result = mapper.Map<TDestination>(source);
```



### 构建器 ( Builder )

在大多数情况下，`Adapt`方法就足够了，但有时我们需要构建器来支持一些特殊的场景。

一个基本的例子 —— 传递运行时的值：

```csharp
var dto = poco.BuildAdapter()
              .AddParameters("user", this.User.Identity.Name)
              .AdaptToType<SimpleDto>();
```

如果你使用mapper实例，你可以通过 `From` 创建构建器。

```csharp
var dto = mapper.From(poco)
              .AddParameters("user", this.User.Identity.Name)
              .AdaptToType<SimpleDto>();
```



### 代码生成

请参阅 [Mapster.Tool](Mapster.Tool.md)来 生成特定的映射器类，而不是使用提供的映射器。

