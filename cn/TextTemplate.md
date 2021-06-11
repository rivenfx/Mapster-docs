# TextTemplate

### TextTemplate
```
    PM> Install-Package ExpressionTranslator
```

Although Mapster allow you to [step-into debugging](https://github.com/MapsterMapper/Mapster/wiki/Debugging), but all mapping are generated at runtime. Therefore, error will be captured at runtime, and we also lose the power of static analysis to find the usage.

Here are steps to add code generation.

1. Create text template

![image](https://user-images.githubusercontent.com/5763993/56052976-f9377580-5d7c-11e9-841c-0a911fdb3a7f.png)

2. In template, add references & mapping logic

```xml
<#@ template debug="true" language="C#" #>
<#@ output extension=".g.cs" #>
<#@ Assembly Name="netstandard" #>
<#@ Assembly Name="System.Core" #>
<#@ Assembly Name="System.Runtime" #>
<#@ Assembly Name="System.Linq.Expressions" #>
<#@ Assembly Name="$(TargetDir)/$(ProjectName).dll" #>
<#@ Assembly Name="$(TargetDir)/Mapster.dll" #>
<#@ Assembly Name="$(TargetDir)/ExpressionTranslator.dll" #>
<#@ import namespace="ExpressionDebugger" #>
<#@ import namespace="Mapster" #>
<#@ import namespace="YourNamespace" #>
```
```csharp
<# 
    //this line is to generate all nested mapping in 1 file
    TypeAdapterConfig.GlobalSettings.SelfContainedCodeGeneration = true;
    var cust = default(Customer);
    var def = new ExpressionDefinitions
    {
        IsStatic = true,    //change to false if you want instance
        MethodName = "Map",
        Namespace = "YourNamespace",
        TypeName = "CustomerMapper"
    };
    var code = cust.BuildAdapter()
        .CreateMapExpression<CustomerDTO>()
        .ToScript(def);
    WriteLine(code);
#>
```

3. Generate code by right click on template file, and select `Run Custom Tool`.

That's it. Done!

---

## Information

### Links

- Example: [CustomerMapper](
https://github.com/MapsterMapper/Mapster/blob/master/src/Benchmark/CustomerMapper.tt)
- [T4 Documentation](https://docs.microsoft.com/en-us/visualstudio/modeling/code-generation-and-t4-text-templates?view=vs-2019) (Microsoft)

### Q & A

Q: How to pass lambda to Before/After mapping?  
A: Please use `BeforeMappingInline` and `AfterMappingInline` instead. [link](https://github.com/MapsterMapper/Mapster/wiki/Before-after-mapping)

Q: Can I generate multiple outputs from single template?  
A: You can. [link](https://stackoverflow.com/questions/33575419/how-to-create-multiple-output-files-from-a-single-t4-template-using-tangible-edi)

Q: After running template file, it said library XXX not found.  
A: Some unused libraries will be excluded during build. You can direct reference to dll in template file. Or tell Visual Studio to copy all reference libraries to output. [link](https://stackoverflow.com/questions/43837638/how-to-get-net-core-projects-to-copy-nuget-references-to-build-output/43841481)

```xml
<!-- This setting will copy all references to output -->
<CopyLocalLockFileAssemblies>true</CopyLocalLockFileAssemblies>

<!-- This setting apply only Debug build -->
<CopyLocalLockFileAssemblies Condition=" '$(Configuration)'=='Debug' ">true</CopyLocalLockFileAssemblies>
```

Q: After running template file on Mac, it said `netstandard` is not found.  
A: You need direct reference.

```xml
<!-- Remove this line -->
<#@ Assembly Name="netstandard" #>

<!-- Change to this line (path might not be the same) -->
<#@ Assembly Name="/usr/local/share/dotnet/sdk/2.2.103/Microsoft/Microsoft.NET.Build.Extensions/net461/lib/netstandard.dll" #>
```

Q: After running template file in .NET Core project on Windows, it said, System.Runtime version 4.2.x.x not found.  
A: You can build using .NET Framework version. Otherwise, you need to update assembly binding in Visual Studio config file. [link](https://stackoverflow.com/questions/51550265/t4-template-could-not-load-file-or-assembly-system-runtime-version-4-2-0-0)

Q: After running template file, it said Compile items are duplicated.  
A: You can set property to skip generated items.

```xml
<DefaultItemExcludes>**/*.g.cs</DefaultItemExcludes>
```