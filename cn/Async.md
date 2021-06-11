# Async

### Async support

    PM> Install-Package Mapster.Async

This plugin allows you to perform async operation for mapping

##### Setup

Use `AfterMappingAsync` to setup async operation

```csharp
config.NewConfig<Poco, Dto>()
    .AfterMappingAsync(async (poco, dto) =>
    {
        var userManager = MapContext.Current.GetService<UserManager>();
        var user = await userManager.FindByIdAsync(poco.UserId);
        dto.UserName = user.Name;
    });
```

##### Mapping

Then map asynchronously with `AdaptToTypeAsync`.

```csharp
var dto = await poco.BuildAdapter()
    .AdaptToTypeAsync<Dto>();
```


Or like this, if you use mapper instance.

```csharp
var dto = await _mapper.From(poco)
    .AdaptToTypeAsync<Dto>();
```