# Fluent API 代码生成

### 配置类型

创建一个实现 `ICodeGenerationRegister` 接口的类：

```csharp
public class MyRegister : ICodeGenerationRegister
{
    public void Register(CodeGenerationConfig config)
    {
        config.AdaptTo("[name]Dto")
            .ForAllTypesInNamespace(Assembly.GetExecutingAssembly(), "Sample.CodeGen.Domains");

        config.GenerateMapper("[name]Mapper")
                .ForType<Course>()
                .ForType<Student>();
    }
}
```

### 生成模型

声明 `AdaptFrom`, `AdaptTo`, 或 `AdaptTwoWays`.

例如:
```csharp
config.AdaptTo("[name]Dto")
    ForType<Student>();
```

然后Mapster会生成:
```csharp
public class StudentDto {
    ...
}
```

#### 添加要生成的类型

可以通过 `ForTypes`, `ForAllTypesInNamespace`, `ForType<>` 方法添加要生成类型，并且可以通过 `ExcludeTypes` 方法过滤不生成的类型：

```csharp
config.AdaptTo("[name]Dto")
    .ForAllTypesInNamespace(Assembly.GetExecutingAssembly(), "Sample.CodeGen.Domains")
    .ExcludeTypes(typeof(SchoolContext))
    .ExcludeTypes(type => type.IsEnum)
```


#### 在生成时忽略一些成员

默认代码生成将忽略标记了 `[AdaptIgnore]` 特性的成员，但是可以通过 `IgnoreAttributes`, `IgnoreNoAttributes`, `IgnoreNamespaces` 方法自定义生成代码忽略成员的逻辑。

例如，使用  `IgnoreNoAttributes` 方法配置让没有 `[DataMember]` 特性标记的成员在生成时忽略：
```csharp
config.AdaptTo("[name]Dto")
    .ForType<Student>()
    .IgnoreNoAttributes (typeof(DataMemberAttribute));

public class Student {
    [DataMember]
    public string Name { get; set; }     // 这个属性将生成到DTO
    public string LastName { get; set; } // 这个属性将不会生成到DTO
}
```

#### 忽略属性

```csharp
config.AdaptTo("[name]Dto")
    .ForType<Student>(cfg => {
        cfg.Ignore(poco => poco.LastName);
    });
```

#### 更改属性名称、类型

```csharp
config.AdaptTo("[name]Dto")
    .ForType<Student>(cfg => {
        cfg.Map(poco => poco.LastName, "Surname");   // 修改属性名称
        cfg.Map(poco => poco.Grade, typeof(string)); // 修改属性类型
    });
```

#### 转发属性类型 

默认情况下，代码生成将在同一声明上转发类型。例如，`Student` 有`ICollection<Enrollment>`，代码生成后`StudentDto` 将有`ICollection<EnrollmentDto>`。

可以通过 `AlterType` 方法进行重写：

```csharp
config.AdaptTo("[name]Dto")
    .ForAllTypesInNamespace(Assembly.GetExecutingAssembly(), "Sample.CodeGen.Domains")
    .AlterType<Student, Person>();    // 转发 Student 到 Person
```

#### 生成只读属性

对于`AdaptTo` 和`AdaptTwoWays`，可以使用`MapToConstructor` 设置生成只读属性：

```csharp
config.AdaptTo("[name]Dto")
    .ForType<Student>()
    .MapToConstructor(true);
```

这会生成：
```csharp
public class StudentDto {
    public string Name { get; }

    public StudentDto(string name) {
        this.Name = name;
    }
}
```

#### 生成可空属性

对于 `AdaptFrom`，可以使用 `IgnoreNullValues` 设置生成可为空的属性 :

```csharp
config.AdaptFrom("[name]Merge")
    .ForType<Student>()
    .IgnoreNullValues(true);
```

这会生成：
```csharp
public class StudentMerge {
    public int? Age { get; set; }
}
```

### 生成扩展方法

#### 使用 `GenerateMapper` 生成
对于任何用`AdaptFrom`， `AdaptTo`或 `AdaptTwoWays` 定义的实体类，可以通过 `GenerateMapper` 方法来生成扩展方法：

```csharp
config.AdaptTo("[name]Dto")
    .ForType<Student>();

config.GenerateMapper("[name]Mapper")
    .ForType<Student>();
```

上面的配置会让Mapster生成扩展方法静态类：
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