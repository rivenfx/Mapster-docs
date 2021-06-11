# Interface base Code generation

### Generate mapper from interface

Annotate your interface with `[Mapper]` in order for tool to pickup for generation.

This is example interface.
```csharp
[Mapper]
public interface IProductMapper
{
    ProductDTO Map(Product customer);
}
```

You can add multiple members as you want. All member names are flexible, but signature must be in following patterns:
```csharp
[Mapper]
public interface ICustomerMapper
{
    //for queryable
    Expression<Func<Customer, CustomerDTO>> ProjectToDto { get; }
    
    //map from POCO to DTO
    CustomerDTO MapToDto(Customer customer);

    //map to existing object
    Customer MapToExisting(CustomerDTO dto, Customer customer);
}
```

If you have configuration, it must be in `IRegister`

```csharp
public class MyRegister : IRegister
{
    public void Register(TypeAdapterConfig config)
    {
        config.NewConfig<TSource, TDestination>();
    }
}
```