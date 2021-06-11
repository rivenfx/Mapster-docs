# 自定义映射

### 自定义成员的映射关系

Mapster 支持自定义映射某个属性的值：

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Map(dest => dest.FullName,
        src => string.Format("{0} {1}", src.FirstName, src.LastName));
```



也可以在源属性类型和目标属性类型不同时进行映射：

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Map(dest => dest.Gender,      // dest.Gender： Genders.Male 或 Genders.Female 枚举类型
        src => src.GenderString); // src.GenderString： "Male" 或 "Female" 字符串类型
```

### 根据条件映射

可以通过设置 `Map` 方法的第三个参数，实现在 源对象 满足某些条件下进行映射；

当存在多个条件的情况下，Mapster 会依次往下执行判断条件是否满足，直到满足条件为止；

当找不到满足条件的映射时，将分配空值或默认值：

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Map(dest => dest.FullName, src => "Sig. " + src.FullName, srcCond => srcCond.Country == "Italy")
    .Map(dest => dest.FullName, src => "Sr. " + src.FullName, srcCond => srcCond.Country == "Spain")
    .Map(dest => dest.FullName, src => "Mr. " + src.FullName);
```

> 注意！如果想要在满足条件时跳过映射，应该使用 `IgnoreIf`，详情可参阅 [忽略成员](Ignoring-members.md#ignore-conditionally)

### 映射非公开成员

如果存在私有成员需要映射，那么可以使用 `Map` 方法指定成员名称实现私有成员映射：

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .Map("PrivateDestName", "PrivateSrcName");
```

> 更多有关于映射非公开成员的资料，请参阅 [映射非公开成员](Mapping-non-public-members.md) 

### 目标多级属性映射

使用 `Map` 方法可以为 目标多级别属性配置映射：

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
    .Map(dest => dest.Child.Name, src => src.Name);
```

### 空映射

如果 `src.Child` 为 `null`，那么映射到 `dest.Name` 的配置不会抛出  `NullPointerException` ，而是映射空值：

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
    .Map(dest => dest.Name, src => src.Child.Name);
```



### 多个源



**映射 Dto 中属性的值到 Poco**

```csharp
public class Poco
{
    public string Name { get; set; }
    public string Extra { get; set; }
}

public class Dto
{
    public string Name { get; set; }
    public SubDto SubDto { get; set; }
}
public class SubDto
{
    public string Extra { get; set; }
}
```

如果想将 `Dto` 中的所有属性和 `Dto.SubDto` 中的所有属性映射到 `Poco`，那么可以通过配置 `dto.SubDto` 映射到 `Poco` 来实现：

```csharp
TypeAdapterConfig<Dto, Poco>.NewConfig()
    .Map(poco => poco, dto => dto.SubDto);
```



**映射两个类型到一个类型**

如果想将 `Dto1`与`Dto2`两个类型映射到  `Poco` 类型，那么可以通过将 `Dto1` `Dto2` 包装成一个 tuple，然后将 `tuple.Item1` 和 `tuple.Item2` 映射到 `Poco` 来实现：

```csharp
TypeAdapterConfig<(Dto1, Dto2), Poco>.NewConfig()
    .Map(dest => dest, src => src.Item1)
    .Map(dest => dest, src => src.Item2);
```