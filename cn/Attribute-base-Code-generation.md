# 基于 Attribute 生成代码

### 生成模型

在类上标记 `[AdaptFrom]`, `[AdaptTo]`, 或 `[AdaptTwoWays]`：

```csharp
[AdaptTo("[name]Dto")]
public class Student {
    ...
}
```

这将会生成：
```csharp
public class StudentDto {
    ...
}
```

#### 在生成时忽略一些成员

默认代码生成将忽略标记了 `[AdaptIgnore]` 特性的成员，但是可以通过 `IgnoreAttributes`, `IgnoreNoAttributes`, `IgnoreNamespaces` 特性标记自定义生成代码忽略成员的逻辑。

例如，使用  `IgnoreNoAttributes` 特性配置让没有 `[DataMember]` 特性标记的成员在生成时忽略：
```csharp
[AdaptTo("[name]Dto", IgnoreNoAttributes = new[] { typeof(DataMemberAttribute) })]
public class Student {

    [DataMember]
    public string Name { get; set; }     // 这个属性将生成到DTO
    
    public string LastName { get; set; }  // 这个属性将不会生成到DTO
}
```

#### 修改属性类型

默认情况下，代码生成将在同一声明上转发类型。例如，`Student` 有`ICollection<Enrollment>`，代码生成后`StudentDto` 将有`ICollection<EnrollmentDto>`。

可以通过 `[PropertyType(typeof(Target))]` 特性标记重写默认行为

>  这个特性标记既可以注释到属性上，也可以特性标记到类上。

例：
```csharp
[AdaptTo("[name]Dto")]
public class Student {
    public ICollection<Enrollment> Enrollments { get; set; }
}

[AdaptTo("[name]Dto"), PropertyType(typeof(DataItem))]
public class Enrollment {
    [PropertyType(typeof(string))]
    public Grade? Grade { get; set; }
}
```

这将生成:
```csharp
public class StudentDto {
    public ICollection<DataItem> Enrollments { get; set; }
}
public class EnrollmentDto {
    public string Grade { get; set; }
}
```

#### 生成只读属性

对于 `[AdaptTo]` 和 `[AdaptTwoWays]` 特性标记，可以通过设置 `MapToConstructor`来生成只读属性:

```csharp
[AdaptTo("[name]Dto", MapToConstructor = true)]
public class Student {
    public string Name { get; set; }
}
```

这将生成：
```csharp
public class StudentDto {
    public string Name { get; }

    public StudentDto(string name) {
        this.Name = name;
    }
}
```

#### 生成可空属性

对于 `[AdaptFrom]` 特性标记，可以通过设置 `IgnoreNullValues`来生成可空属性:

```csharp
[AdaptFrom("[name]Merge", IgnoreNullValues = true)]
public class Student {
    public int Age { get; set; }
}
```

这将生成：
```csharp
public class StudentMerge {
    public int? Age { get; set; }
}
```

### 生成扩展方法

#### 使用 [GenerateMapper] 特性标记
对于任何带有 `[AdaptFrom]`， `[AdaptTo]` 或 `[AdaptTwoWays]` 特性标记的实体类，可以通过添加 `[GenerateMapper]` 特性标记实现生成扩展方法：

```csharp
[AdaptTo("[name]Dto"), GenerateMapper]
public class Student {
    ...
}
```

这将生成：
```csharp
public class StudentDto {
    ...
}
public static class StudentMapper {
    public static StudentDto AdaptToDto(this Student poco) { ... }
    public static StudentDto AdaptTo(this Student poco, StudentDto dto) { ... }
    public static Expression<Func<Student, StudentDto>> ProjectToDto => ...
}
```

#### 配置
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

#### 使用配置生成

可以生成扩展方法并从映射配置中添加额外的设置。

```csharp
public class MyRegister : IRegister
{
    public void Register(TypeAdapterConfig config)
    {
        config.NewConfig<TSource, TDestination>()
            .GenerateMapper(MapType.Map | MapType.MapToTarget);
    }
}
```