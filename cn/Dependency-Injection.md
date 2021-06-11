# Dependency Injection



### Dependency injection support

    PM> Install-Package Mapster.DependencyInjection

This plugin allows you to inject service into mapping configuration.

#### Usage

On startup, register `TypeAdapterConfig`, and `ServiceMapper`.

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

NOTE: lifetime of `ServiceMapper` is up to services you would like to inject. It can be singleton, if you inject only singleton services. Or it can be transient, if any injected services is transient.

##### Mapping configuration

You can get service by `MapContext.Current.GetService<>()`, for example

```csharp
config.NewConfig<Poco, Dto>()
    .Map(dest => dest.Name, src => MapContext.Current.GetService<INameFormatter>().Format(src.Name));
```

##### Mapping

If you setup service injection, you need to use mapper instance to map object.

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