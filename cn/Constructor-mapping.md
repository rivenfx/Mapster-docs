# 构造函数映射



### 自定义目标对象创建 

Mapster 默认实例化目标类型时，会使用目标类型的默认空构造函数实例化对象。

而使用 `ConstructUsing` 可以自定义实例化目标类型的实现：

```csharp
// 使用非默认构造函数
TypeAdapterConfig<TSource, TDestination>.NewConfig()
    .ConstructUsing(src => new TDestination(src.Id, src.Name));

// 使用对象初始化器
TypeAdapterConfig<TSource, TDestination>.NewConfig()
    .ConstructUsing(src => new TDestination{Unmapped = "unmapped"});
```

### 映射到构造函数

Mapster 默认情况下只映射到字段和属性。可以通过 `MapToConstructor` 配置映射到构造函数：

```csharp
// 全局
TypeAdapterConfig.GlobalSettings.Default.MapToConstructor(true);

// 特定类型
TypeAdapterConfig<Poco, Dto>.NewConfig().MapToConstructor(true);
```

如果想要定义自定义映射，你需要使用 Pascal case:

```csharp
class Poco {
    public string Id { get; set; }
    ...
}
class Dto {
    public Dto(string code, ...) {
        ...
    }
}
```
```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
    .MapToConstructor(true)
    .Map('Code', 'Id'); //use Pascal case
```

如果一个类有多个构造函数，Mapster 将自动选择满足映射的最大数量的参数的构造函数：

```csharp
class Poco {
    public int Foo { get; set; }
    public int Bar { get; set; }
}
class Dto {
    public Dto(int foo) { ... }
    public Dto(int foo, int bar) { ...} //<-- Mapster 将使用这个构造函数
    public Dto(int foo, int bar, int baz) { ... }
}
```
或者也可以显式地传递 ConstructorInfo 给 `MapToConstructor` 方法:

```csharp
var ctor = typeof(Dto).GetConstructor(new[] { typeof(int), typeof(int) });
TypeAdapterConfig<Poco, Dto>.NewConfig()
    .MapToConstructor(ctor);
```

