# 映射配置继承



### 隐式继承映射配置



#### 源类型

Mapster 默认会把 源类型的 映射配置 应用到 源类型的子类。

如果你创建了一个 `SimplePoco` -> `SimpleDto` 的映射配置:

```csharp
TypeAdapterConfig<SimplePoco, SimpleDto>.NewConfig()
    .Map(dest => dest.Name, src => src.Name + "_Suffix");
```

那么继承了 `SimplePoco`  的 `DerivedPoco` 也将应用同样的映射配置：

```csharp
var dest = TypeAdapter.Adapt<DerivedPoco, SimpleDto>(src); 
//dest.Name = src.Name + "_Suffix"
```

如果不希望子类使用父类映射配置，可以设置 `AllowImplicitSourceInheritance` 关闭：

```csharp
TypeAdapterConfig.GlobalSettings.AllowImplicitSourceInheritance = false;
```



#### 目标类型

Mapster 默认不会把 目标类型的 映射配置 应用到 目标类型的子类。

可以设置 `AllowImplicitDestinationInheritance` 开启:

```csharp
TypeAdapterConfig.GlobalSettings.AllowImplicitDestinationInheritance = true;
```





### 显示继承映射配置

可以通过 `Inherits` 方法显示的继承类型映射配置：

```csharp
TypeAdapterConfig<DerivedPoco, DerivedDto>.NewConfig()
        .Inherits<SimplePoco, SimpleDto>();
```



### 子类继承父类映射配置

可以通过 `Include` 方法显示的让子类继承父类的映射配置：

```csharp
TypeAdapterConfig<Vehicle, VehicleDto>.NewConfig()
    .Include<Car, CarDto>();

Vehicle vehicle = new Car { Id = 1, Name = "Car", Make = "Toyota" };
var dto = vehicle.Adapt<Vehicle, VehicleDto>();

dto.ShouldBeOfType<CarDto>();
((CarDto)dto).Make.ShouldBe("Toyota"); //The 'Make' property doesn't exist in Vehicle
```