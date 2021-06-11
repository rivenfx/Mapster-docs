# 特性标签



### AdaptIgnore 特性

当一个属性有 `[AdaptIgnore]` 标记时，这个属性将不会被映射：

```csharp
public class Product {
    public string Id { get; set; }
    public string Name { get; set; }

    [AdaptIgnore]
    public decimal Price { get; set; }
}
```

当一个成员有 `[AdaptIgnore]` 标记时，不管是 源到目标 还是 目标到源 的映射都将会忽略这个成员，可以使用 `MemberSide` 指定单方的忽略。

例如，只有 `Product` 当作 源映射时，`Price` 字段才会被忽略：

```csharp
public class Product {
    public string Id { get; set; }
    public string Name { get; set; }

    [AdaptIgnore(MemberSide.Source)]
    public decimal Price { get; set; }
}
```

### 忽略自定义的 Attribute

Mapster 还支持忽略标记了任何 Attribute 的属性，使用 `IgnoreAttribute`  方法指定要忽略的 Attribute 。

例如，忽略所有标记了 `JsonIgnoreAttribute` 的属性：

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IgnoreAttribute(typeof(JsonIgnoreAttribute));
```

`IgnoreAttribute`  的映射配置会在 源到目标 和 目标到源 时生效，如果只想在单方生效，可以使用 `IgnoreMember` 方法实现：

```csharp
config.IgnoreMember((member, side) => member.HasCustomAttribute(typeof(NotMapAttribute)) && side == MemberSide.Source);
```



### AdaptMember 特性标记

**映射到不同的名称**

使用 `[AdaptMember]` 特性标记可以实现修改映射的名称。

例如，将 `Id` 映射为 `Code`:

```csharp
public class Product {
    [AdaptMember("Code")]
    public string Id { get; set; }
    public string Name { get; set; }
}
```

**映射非公开成员**  

使用 `[AdaptMember]` 特性标记可以实现映射非公开成员：

```csharp
public class Product {
    [AdaptMember]
    private string HiddenId { get; set; }
    public string Name { get; set; }
}
```

### 根据自定义属性重命名成员

Mapster 支持重写成员的名称，通过 `GetMemberName` 方法可实现。

例如，通过在类的属性上标记 `JsonProperty` 特性指定属性名称：

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .GetMemberName(member => member.GetCustomAttributes(true)
                                    .OfType<JsonPropertyAttribute>()
                                    .FirstOrDefault()?.PropertyName);  //if return null, property will not be renamed
```

>  注意！如果 `GetMemberName` 返回结果为空，那么将不会重写成员名称





### 包括自定义 Attribute

可以使用  `IncludeAttribute` 实现根据成员拥有的标记特性来映射非公开的成员。

例如，映射所有标记了 `JsonPropertyAttribute` 的非公开成员：

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IncludeAttribute(typeof(JsonPropertyAttribute));
```



### 目标值

 `[UseDestinationValue]` 特性标记可以让 Mapster 使用现有的值来做映射，而不是创建实例化新对象。

例如下面的例子，`Items` 属性默认为 `new List<OrderItem>()`，使用 `UseDestinationValue` 后 `Items` 在映射时不会创建新对象，而是直接使用已经实例化的集合对象：

```csharp
public class Order {
    public string Id { get; set; }

    [UseDestinationValue]
    public ICollection<OrderItem> Items { get; } = new List<OrderItem>();
}
```