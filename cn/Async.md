# 异步支持

### 安装 nuget 包

> 这个插件允许为映射执行异步操作

    PM> Install-Package Mapster.Async

##### 配置

使用 `AfterMappingAsync`方法配置映射完成后的处理过程：

```csharp
config.NewConfig<Poco, Dto>()
    .AfterMappingAsync(async (poco, dto) =>
    {
        var userManager = MapContext.Current.GetService<UserManager>();
        var user = await userManager.FindByIdAsync(poco.UserId);
        dto.UserName = user.Name;
    });
```

##### 映射

然后使用 `AdaptToTypeAsync` 进行异步映射:

```csharp
var dto = await poco.BuildAdapter()
    .AdaptToTypeAsync<Dto>();
```


如果使用 `IMapper` 实例的话，可以像下面的例子这样进行异步映射：

```csharp
var dto = await _mapper.From(poco)
    .AdaptToTypeAsync<Dto>();
```