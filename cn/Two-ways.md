# Two ways

### 2-ways mapping

If you need to map object from POCO to DTO, and map back from DTO to POCO. You can define the setting once by using `TwoWays`.

```csharp
TypeAdapterConfig<Poco, Dto>
    .NewConfig()
    .TwoWays()
    .Map(dto => dto.Code, poco => poco.Id); //<-- this setting will apply dto.Code = poco.Id & poco.Id = dto.Code for reverse mapping
```

NOTE: `TwoWays` command need to call before setting to take effect.

```csharp
TypeAdapterConfig<Poco, Dto>
    .NewConfig()
    .Map(dto => dto.Foo, poco => poco.Bar)  //<-- this map only apply to Poco->Dto
    .TwoWays()
    .Map(dto => dto.Foz, poco => poco.Baz); //<-- this map will apply both side
```

### Unflattening

By default, Mapster will perform flattening.

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

Above example, without any setup, you can map from POCO to DTO and you will get `SupervisorName` from `Supervisor.Name` (flattening).

However, unflattening process needed to be defined. You can map to `Supervisor.Name` from `SupervisorName` by 

#### using `Unflattening`.

```csharp
TypeAdapterConfig<StaffDto, Staff>.NewConfig()
    .Unflattening(true);
```

#### using `TwoWays`

```csharp
TypeAdapterConfig<Staff, StaffDto>
    .NewConfig()
    .TwoWays(); //<-- this will also map poco.Supervisor.Name = dto.SupervisorName for reverse mapping
```