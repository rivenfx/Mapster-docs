# 映射类型



### 基本类型

基本类型的转换 ，例如： `int/bool/dobule/decimal`  ，包括可空的基本类型。

只要C#支持类型转换的类型，那么在 Mapster 中也同样支持转换。

```csharp
decimal i = 123.Adapt<decimal>(); //equal to (decimal)123;
```


### 枚举类型

Mapster 会自动把枚举映射到数字类型，同样也支持 字符串到枚举 和 枚举到字符串的映射。

.NET 默认实现 枚举/字符串 转换非常慢，Mapster 比 .NET 的默认实现快两倍。

在 Mapster 中，字符串转枚举，如果字符串为空或空字符串，那么枚举将初始化为第一个枚举值。

在Mapster中，也支持标记的枚举。

```csharp
var e = "Read, Write, Delete".Adapt<FileShare>();  
//FileShare.Read | FileShare.Write | FileShare.Delete
```
对于不同类型的枚举，Mapster 默认将值映射为枚举。调用 `EnumMappingStrategy` 方法可以指定枚举映射方式，如：

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .EnumMappingStrategy(EnumMappingStrategy.ByName);
```


### 字符串类型

在 Mapster 中，将其它类型映射为字符串时，Mapster 将调用类型的 `ToString` 方法。

如果将字符串映射为类型时，Mapster 将调用类型的 `Parse` 方法。

```csharp
var s = 123.Adapt<string>(); // 等同于: 123.ToString();
var i = "123".Adapt<int>();  // 等同于: int.Parse("123");
```


### 集合

包括列表、数组、集合、包括各种接口的字典之间的映射: `IList<T>`, `ICollection<T>`, `IEnumerable<T>`, `ISet<T>`, `IDictionary<TKey, TValue>` 等等…

```csharp
var list = db.Pocos.ToList();
var target = list.Adapt<IEnumerable<Dto>>();  
```



### 可映射对象

Mapster 可以使用以下规则映射两个不同的对象

- 源类型和目标类型属性名称相同。 例如: `dest.Name = src.Name`
- 源类型有 `GetXXXX` 方法。例如: `dest.Name = src.GetName()`
- 源类型属性有子属性，可以将子属性的赋值给符合条件的目标类型属性，例如: `dest.ContactName = src.Contact.Name` 或 `dest.Contact_Name = src.Contact.Name`

示例:
```csharp
class Staff {
    public string Name { get; set; }
    public int GetAge() { 
        return (DateTime.Now - this.BirthDate).TotalDays / 365.25; 
    }
    public Staff Supervisor { get; set; }
    ...
}

struct StaffDto {
    public string Name { get; set; }
    public int Age { get; set; }
    public string SupervisorName { get; set; }
}

var dto = staff.Adapt<StaffDto>();  
//dto.Name = staff.Name, dto.Age = staff.GetAge(), dto.SupervisorName = staff.Supervisor.Name
```

可映射对象类型包括:
- 类
- 结构体
- 接口
- 实现 `IDictionary<string, T>` 接口的字典类型
- Record 类型 (类、结构体、接口)

对象转换为字典的例子：

```csharp
var point = new { X = 2, Y = 3 };
var dict = point.Adapt<Dictionary<string, int>>();
dict["Y"].ShouldBe(3);
```



Record 类型的例子：	

```csharp
class Person {
    public string Name { get; }
    public int Age { get; }

    public Person(string name, int age) {
        this.Name = name;
        this.Age = age;
    }
}

var src = new { Name = "Mapster", Age = 3 };
var target = src.Adapt<Person>();
```

自动映射 Record 类型有一些限制：

* Record 类型属性必须没有 `set`
* 只有一个非空构造函数
* 构造函数中的所有参数名称必须与属性名称相同

如果不符合以上规则，需要增加额外的 [`MapToConstructor`](Constructor-mapping.md#map-to-constructor)  配置