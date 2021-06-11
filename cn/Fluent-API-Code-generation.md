# Fluent API Code generation

### Configuration class

Create a configuration class implement `ICodeGenerationRegister`.

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

### Generate models

Declare `AdaptFrom`, `AdaptTo`, or `AdaptTwoWays`.

Example:
```csharp
config.AdaptTo("[name]Dto")
    ForType<Student>();
```

Then Mapster will generate:
```csharp
public class StudentDto {
    ...
}
```

#### Add types to generate

You can add types by `ForTypes`, `ForAllTypesInNamespace`, `ForType<>`, and you can remove added types using `ExcludeTypes`.
```csharp
config.AdaptTo("[name]Dto")
    .ForAllTypesInNamespace(Assembly.GetExecutingAssembly(), "Sample.CodeGen.Domains")
    .ExcludeTypes(typeof(SchoolContext))
    .ExcludeTypes(type => type.IsEnum)
```


#### Ignore some properties on generation

By default, code generation will ignore properties that annotated `[AdaptIgnore]` attribute. But you can add more settings which include `IgnoreAttributes`, `IgnoreNoAttributes`, `IgnoreNamespaces`.

Example:
```csharp
config.AdaptTo("[name]Dto")
    .ForType<Student>()
    .IgnoreNoAttributes (typeof(DataMemberAttribute));

public class Student {
    [DataMember]
    public string Name { get; set; }     //this property will be generated
    public string LastName { get; set; } //this will not be generated
}
```

#### Ignore a property

```csharp
config.AdaptTo("[name]Dto")
    .ForType<Student>(cfg => {
        cfg.Ignore(poco => poco.LastName);
    });
```

#### Change a property name, type

```csharp
config.AdaptTo("[name]Dto")
    .ForType<Student>(cfg => {
        cfg.Map(poco => poco.LastName, "Surname");   //change property name
        cfg.Map(poco => poco.Grade, typeof(string)); //change property type
    });
```

#### Forward property types

By default, code generation will forward type on the same declaration. (For example, `Student` has `ICollection<Enrollment>`, after code generation `StudentDto` will has `ICollection<EnrollmentDto>`).

You can override this by `AlterType`.

```csharp
config.AdaptTo("[name]Dto")
    .ForAllTypesInNamespace(Assembly.GetExecutingAssembly(), "Sample.CodeGen.Domains")
    .AlterType<Student, Person>();    //forward all Student to Person
```

#### Generate readonly properties

For `AdaptTo` and `AdaptTwoWays`, you can generate readonly properties with `MapToConstructor` setting.

For example:
```csharp
config.AdaptTo("[name]Dto")
    .ForType<Student>()
    .MapToConstructor(true);
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

For `AdaptFrom`, you can generate nullable properties with `IgnoreNullValues` setting.

For example:
```csharp
config.AdaptFrom("[name]Merge")
    .ForType<Student>()
    .IgnoreNullValues(true);
```

This will generate:
```csharp
public class StudentMerge {
    public int? Age { get; set; }
}
```

### Generate extension methods

#### Generate using `GenerateMapper`.
For any POCOs declared with `AdaptFrom`, `AdaptTo`, or `AdaptTwoWays`, you can declare `GenerateMapper` in order to generate extension methods.

Example:
```csharp
config.AdaptTo("[name]Dto")
    .ForType<Student>();

config.GenerateMapper("[name]Mapper")
    .ForType<Student>();
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