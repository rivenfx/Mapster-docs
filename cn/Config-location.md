### 入口

映射配置应该只初始化并且只进行一次配置。因此在编写代码的时候不能将映射配置和映射调用放在同一个地方。

例如下面的例子，运行将会抛出一个异常:

```csharp
config.ForType<Poco, Dto>().Ignore("Id");
var dto1 = poco1.Adapt<Dto>(config);

config.ForType<Poco, Dto>().Ignore("Id"); //<--- 这里将抛出异常，因为在这之前已经触发过了映射
var dto2 = poco2.Adapt<Dto>(config);
```

所以，在编写代码时应该将映射配置和映射调用分开，将映射配置放到程序的入口，例如： 

* `Main` 方法
*  `Global.asax.cs` 
*  `Startup.cs`

```csharp
// Application_Start in Global.asax.cs
config.ForType<Poco, Dto>().Ignore("Id");
```

```csharp
// in Controller class
var dto1 = poco1.Adapt<Dto>(config);
var dto2 = poco2.Adapt<Dto>(config);
```



### 保证配置实例和映射配置在同一位置

将配置实例和映射配置分开，可能会导致代码处于不同的位置，在代码编写过程中可能会出现遗漏之类的问题。

使用 `Fork` 函数可以使配置实例与映射配置在同一位置：

```csharp
var dto = poco.Adapt<Dto>(
	config.Fork(forked => forked.ForType<Poco, Dto>().Ignore("Id"));
```

> 不用担心性能问题，只有第一次使用配置实例时会编译，之后的调用将从缓存中获取。

##### 在泛型类或方法中使用 Fork

`Fork` method uses filename and line number and the key. But if you use `Fork` method inside generic class or method, you must specify your own key (with all type names) to prevent `Fork` to return invalid config from different type arguments.

`Fork` 方法默认使用文件名、行号作为键值。但是如果在在泛型类或泛型方法中调用`Fork`方法，必须指定键和类型全名称，防止 `Fork` 从不同的类型参数返回无效的配置。

```csharp
IQueryable<TDto> GetItems<TPoco, TDto>()
{
    var forked = config.Fork(
        f => f.ForType<TPoco, TDto>().Ignore("Id"), 
        $"MyKey|{typeof(TPoco).FullName}|{typeof(TDto).FullName}");
    return db.Set<TPoco>().ProjectToType<TDto>(forked);
}
```



### 不同的程序集

映射配置处于多个不同的程序集是比较常见的情况。

也许你的域程序集有一些映射到域对象的规则，而你的 web api 有一些特定的规则来映射到您的 api 约定。 

#### Scan 方法

Mapster 支持扫描程序集注册映射配置，可以帮助你快速注册映射配置并减小忘记编码注册配置的机率。

> 在某些特定的情况下可以顺序注册程序集，以达到重写映射配置的目的。



扫描程序集注册映射配置非常简单，只需要在程序集中创建一个或多个 `IRegister` 的实现，然后调用 `TypeAdapterConfig` 实例的 `Scan` 方法即可：

```csharp
public class MyRegister : IRegister
{
	public void Register(TypeAdapterConfig config)
	{
		config.NewConfig<TSource, TDestination>();

		//OR to create or enhance an existing configuration
		config.ForType<TSource, TDestination>();
	}
}
```

使用全局配置实例扫描程序集中的配置注册器：

```csharp
TypeAdapterConfig.GlobalSettings.Scan(assembly1, assembly2, assemblyN)
```

或使用特定的配置实例扫描程序集中的配置注册器:

```csharp
var config = new TypeAdapterConfig();
config.Scan(assembly1, assembly2, assemblyN);
```

#### Apply 方法

如果你使用的是其它 程序集扫描库（如 [MEF](https://docs.microsoft.com/zh-cn/dotnet/framework/mef/) ）, 那么你可以调用配置实例的 `Apply` 方法将映射配置注册器添加到配置实例中：

```csharp
var registers = container.GetExports<IRegister>();
config.Apply(registers);
```

`Apply` 方法允许你选择某些 映射配置注册器 添加到 配置实例 中，而不是像 `Scan` 方法把程序集里的所有 映射配置注册器 添加到 配置实例 中：

```csharp
var register = new MockingRegister();
config.Apply(register);
```



### 特性标签

Mapster 支持通过为类型增加 特性标签 的方式 添加类型映射配置：

```csharp
[AdaptTo(typeof(StudentDto), PreserveReference = true)]
public class Student { 
    ...
}
```