# 双向映射

### 双向映射

如果需要 源到目标 和 目标到源 只进行一次配置，那么可以使用 `ToWays` 方法。

下面的配置的表现为：

* 源到目标时  `dto.Code=poco.Id` 
* 目标到源时 `poco.Id=dto.Code`

```csharp
TypeAdapterConfig<Poco, Dto>
    .NewConfig()
    .TwoWays()
    .Map(dto => dto.Code, poco => poco.Id);
```

注意，`TwoWays` 方法必须在配置映射之前进行调用，否则在调用 `TwoWays` 方法之前的配置为单向映射配置，之后的配置为双向映射配置：

```csharp
TypeAdapterConfig<Poco, Dto>
    .NewConfig()
    .Map(dto => dto.Foo, poco => poco.Bar)  //<-- Poco->Dto 时生效
    .TwoWays()
    .Map(dto => dto.Foz, poco => poco.Baz); //<-- 双向映射生效
```

### 扁平化映射与逆扁平化映射

Mapster 默认将会把符合命名扁平化映射约束的成员进行映射。

例如，从 `Staff` 映射到 `StaffDto` 时，将会把 `Supervisor` 属性的 `Name` 属性映射到 `StaffDto` 的 `SupervisorName` 属性：

```csharp
class Staff {
    public string Name { get; set; }
    public Staff Supervisor { get; set; }
    ...
}

struct StaffDto {
    public string SupervisorName { get; set; }
}
```

但是如果想要 `StaffDot` 映射到 `Staff` 时，把 `SupervisorName` 映射到 `Staff` 的  `Supervisor` 属性的 `Name` 属性，那么就需要使用 `Unflattening` 方法进行显示的映射配置，实现 `poco.Supervisor.Name=dto.SupervisorName` 的效果：

```csharp
TypeAdapterConfig<StaffDto, Staff>.NewConfig()
    .Unflattening(true);
```

#### 使用双向映射配置实现逆扁平化映射

使用 `Toways` 方法可以实现与 `Unflattening` 方法同样的效果：

* 源到目标时 `dto.SupervisorName=poco.Supervisor.Name`

* 目标到源时  `poco.Supervisor.Name=dto.SupervisorName` 

```csharp
TypeAdapterConfig<Staff, StaffDto>
    .NewConfig()
    .TwoWays();
```