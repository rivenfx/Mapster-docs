# 命名约定

### Flexible

Mapster 默认在映射时会区分名称大小写，可以通过修改配置器的 `NameMatchingStrategy`  值为 `NameMatchingStrategy.Flexible` 来支持更加灵活的名称映射规则，这个映射规则支持名称在 `PascalCase`, `camelCase`, `lower_case`, `UPPER_CASE` 之间互相映射：

```csharp
//global
TypeAdapterConfig.GlobalSettings.Default.NameMatchingStrategy(NameMatchingStrategy.Flexible);

//type pair
TypeAdapterConfig<Foo, Bar>.NewConfig().NameMatchingStrategy(NameMatchingStrategy.Flexible);
```

### 忽略大小写

当存在一个名称为 `MiXcAsE`  到名称为  `MixCase` 的映射时，   `NameMatchingStrategy.Flexible` 无法支持，因为它会把 `MiXcAsE` 处理为 `Mi-Xc-As-E`，因此我们应当使用 `NameMatchingStrategy.IgnoreCase` 来实现这个映射：

```csharp
TypeAdapterConfig.GlobalSettings.Default.NameMatchingStrategy(NameMatchingStrategy.IgnoreCase);
```

### 成员名称的 前缀、后缀和替换

如果存在以下的类型：

```c#
public class Poco
{
    public string Name { get; set; }
    public string Desc { get; set; }
    public int Age { get; set; }

}

public class Dto
{
    public string m_Name { get; set; }
    public string m_Desc { get; set; }
    public int m_Age { get; set; }
}
```

可以使用 `Map` 方法配置每个属性的映射，但这样会大大的增加编码工作量。

在这个时候就可以使用 `ConvertSourceMemberName`  配置源类型的成员命名，来实现需求：

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
   .NameMatchingStrategy(NameMatchingStrategy.ConvertSourceMemberName(name => "m_" + name));
```

同样也可以使用 `ConvertDestinationMemberName`  配置目标类型的成员命名来实现需求：

```c#
TypeAdapterConfig<Poco, Dto>.NewConfig()
   .NameMatchingStrategy(NameMatchingStrategy.ConvertDestinationMemberName(name => name.Replace("m_", "")));
```





### IDictionary<string, T> 命名约定

如果想要修改 `Poco` 到 `IDictionary<string, T>` 映射 的字段命名规则为 `camelCase`，那么可以修改 `NameMatchingStrategy` 为   `NameMatchingStrategy.ToCamelCase`。

```c#
TypeAdapterConfig<Poco, Dictionary<string, object>>.NewConfig()
    .NameMatchingStrategy(NameMatchingStrategy.ToCamelCase);
```

如果想要从 `camelCase` 命名规则的 `IDictionary<string, T>`  到 `Poco`，那么可以修改 `NameMatchingStrategy` 为 `NameMatchingStrategy.FromCamelCase`

```csharp
TypeAdapterConfig<Dictionary<string, object>, Poco>.NewConfig()
    .NameMatchingStrategy(NameMatchingStrategy.FromCamelCase);
```

> 注意！映射  `IDictionary<string, T>` 到 `Poco`也可以使用  `Flexible` 或 `IgnoreCase`，在这两种命名约定规则下这两种效率比较低，



### 基于规则的命名

Mapster 支持重写成员的名称，通过 `GetMemberName` 方法可实现。

例如，通过在类的属性上标记 `JsonProperty` 特性指定属性名称：

```c#
public class Poco 
{
    [JsonProperty("code")]
    public string Id { get; set; }
}
```

通过 `GetMemberName`  方法配置读取成员的 `JsonPropertyAttribute` 特性获取属性名称：

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .GetMemberName(member => member.GetCustomAttributes(true)
                                    .OfType<JsonPropertyAttribute>()
                                    .FirstOrDefault()?.PropertyName
    );  //if return null, property will not be renamed
```

> 注意！如果 `GetMemberName` 返回结果为空，那么将不会重写成员名称