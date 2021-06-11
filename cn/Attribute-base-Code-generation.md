# Attribute base Code generation

### Generate models

Annotate your class with `[AdaptFrom]`, `[AdaptTo]`, or `[AdaptTwoWays]`.

Example:
```csharp
[AdaptTo("[name]Dto")]
public class Student {
    ...
}
```

Then Mapster will generate:
```csharp
public class StudentDto {
    ...
}
```

#### Ignore some properties on generation

By default, code generation will ignore properties that annotated `[AdaptIgnore]` attribute. But you can add more settings which include `IgnoreAttributes`, `IgnoreNoAttributes`, `IgnoreNamespaces`.

Example:
```csharp
[AdaptTo("[name]Dto", IgnoreNoAttributes = new[] { typeof(DataMemberAttribute) })]
public class Student {

    [DataMember]
    public string Name { get; set; }     //this property will be generated
    
    public string LastName { get; set; } //this will not be generated
}
```

#### Change property types

By default, if property type annotated with the same adapt attribute, code generation will forward to that type. (For example, `Student` has `ICollection<Enrollment>`, after code generation `StudentDto` will has `ICollection<EnrollmentDto>`).

You can override this by `[PropertyType(typeof(Target))]` attribute. This annotation can be annotated to either on property or on class.

For example:
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

This will generate:
```csharp
public class StudentDto {
    public ICollection<DataItem> Enrollments { get; set; }
}
public class EnrollmentDto {
    public string Grade { get; set; }
}
```

#### Generate readonly properties

For `[AdaptTo]` and `[AdaptTwoWays]`, you can generate readonly properties with `MapToConstructor` setting.

For example:
```csharp
[AdaptTo("[name]Dto", MapToConstructor = true)]
public class Student {
    public string Name { get; set; }
}
```

This will generate:
```csharp
public class StudentDto {
    public string Name { get; }

    public StudentDto(string name) {
        this.Name = name;
    }
}
```

#### Generate nullable properties

For `[AdaptFrom]`, you can generate nullable properties with `IgnoreNullValues` setting.

For example:
```csharp
[AdaptFrom("[name]Merge", IgnoreNullValues = true)]
public class Student {
    public int Age { get; set; }
}
```

This will generate:
```csharp
public class StudentMerge {
    public int? Age { get; set; }
}
```

### Generate extension methods

#### Generate using `[GenerateMapper]` attribute
For any POCOs annotate with `[AdaptFrom]`, `[AdaptTo]`, or `[AdaptTwoWays]`, you can add `[GenerateMapper]` in order to generate extension methods.

Example:
```csharp
[AdaptTo("[name]Dto"), GenerateMapper]
public class Student {
    ...
}
```

Then Mapster will generate:
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

#### Configuration
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

#### Generate using configuration

You can also generate extension methods and add extra settings from configuration.

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