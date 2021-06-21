# 依赖注入



### 依赖注入支持

> 这个插件允许将映射配置添加到依赖注入容器中

    PM> Install-Package Mapster.DependencyInjection



#### 如何使用

在启动时，注册 `TypeAdapterConfig` 和 `ServiceMapper`：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...
    var config = new TypeAdapterConfig();
    // Or
    // var config = TypeAdapterConfig.GlobalSettings;
    services.AddSingleton(config);
    services.AddScoped<IMapper, ServiceMapper>();
    ...
}
```

> 注意！ `ServiceMapper` 可以根据实际的需求来决定在依赖注入容器的生命周期，但 `TypeAdapterConfig` 必须是 `Singleton`。

##### 映射配置

可以通过  `MapContext.Current.GetService<TService>()`  从依赖注入容器中获取服务。

例如从 `MapContext.Current.GetService` 获取 `INameFormatter` 服务：

```csharp
config.NewConfig<Poco, Dto>()
    .Map(dest => dest.Name, src => MapContext.Current.GetService<INameFormatter>().Format(src.Name));
```

##### 映射

如果配置了依赖注入，那么需要注入 `IMapper` 实例用于对象映射：

```csharp
public class FooService {
    private readonly IMapper _mapper;

    public FooService(IMapper mapper) {
        _mapper = mapper;
    }

    public void DoSomething(Poco poco) {
        var dto = _mapper.Map<Dto>(poco);
        ...
    }
}
```