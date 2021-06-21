# 基于接口生成代码

### 从接口生成映射器

给接口添加`[Mapper]` 特性标记，以便让代码生成器获取生成映射器。

例：
```csharp
[Mapper]
public interface IProductMapper
{
    ProductDTO Map(Product customer);
}
```

并且可以根据需要添加多个成员。所有成员的名字都是灵活的，但参数必须使用以下规则:
```csharp
[Mapper]
public interface ICustomerMapper
{
    // 对于 IQueryable
    Expression<Func<Customer, CustomerDTO>> ProjectToDto { get; }
    
    // 映射 Poco 到 DTO
    CustomerDTO MapToDto(Customer customer);

    // 映射到已存在的对象
    Customer MapToExisting(CustomerDTO dto, Customer customer);
}
```

如果有映射配置，它必须在实现了 `IRegister` 的类中：

```csharp
public class MyRegister : IRegister
{
    public void Register(TypeAdapterConfig config)
    {
        config.NewConfig<TSource, TDestination>();
    }
}
```