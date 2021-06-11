# Mapster.Tool

## Mapster.Tool

### Install Mapster.Tool
```bash
#skip this step if you already have dotnet-tools.json
dotnet new tool-manifest 

dotnet tool install Mapster.Tool
```

### Install Mapster
For lightweight dependency, you can just install `Mapster.Core`.
```
PM> Install-Package Mapster.Core
```

However, if you need `TypeAdapterConfig` for advance configuration, you still need `Mapster`.
```
PM> Install-Package Mapster
```

### Commands
Mapster.Tool provides 3 commands
- **model**: generate models from entities
- **extension**: generate extension methods from entities
- **mapper**: generate mappers from interfaces

And Mapster.Tool provides following options
- -a: define input assembly
- -b: specify base namespace for generating dynamic outputs & namespaces
- -n: define namespace of generated classes
- -o: define output directory
- -p: print full type name (if your DTOs/POCOs having the same name)
- -r: generate record types instead of POCO types

### csproj integration

#### Generate manually
add following code to your `csproj` file.
```xml
  <Target Name="Mapster">
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet build" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet tool restore" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster model -a &quot;$(TargetDir)$(ProjectName).dll&quot;" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster extension -a &quot;$(TargetDir)$(ProjectName).dll&quot;" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster mapper -a &quot;$(TargetDir)$(ProjectName).dll&quot;" />
  </Target>
```
to generate run following command on `csproj` file directory:
```bash
dotnet msbuild -t:Mapster
```

#### Generate automatically on build
add following code to your `csproj` file.
```xml
  <Target Name="Mapster" AfterTargets="AfterBuild">
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet tool restore" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster model -a &quot;$(TargetDir)$(ProjectName).dll&quot;" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster extension -a &quot;$(TargetDir)$(ProjectName).dll&quot;" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster mapper -a &quot;$(TargetDir)$(ProjectName).dll&quot;" />
  </Target>
```

#### Clean up
add following code to your `csproj` file.
```xml
  <ItemGroup>
    <Generated Include="**\*.g.cs" />
  </ItemGroup>
  <Target Name="CleanGenerated">
    <Delete Files="@(Generated)" />
  </Target>
```
to clean up run following command:
```bash
dotnet msbuild -t:CleanGenerated
```

#### Generate full type name

If your POCOs and DTOs have the same name, you might need to generate using full type name, by adding `-p` flag.
```xml
  <Target Name="Mapster">
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet build" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet tool restore" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster model -a &quot;$(TargetDir)$(ProjectName).dll&quot; -p" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster extension -a &quot;$(TargetDir)$(ProjectName).dll&quot; -p" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster mapper -a &quot;$(TargetDir)$(ProjectName).dll&quot; -p" />
  </Target>
```

#### Dynamic outputs & namespaces
For example you have following structure.
```
Sample.CodeGen
- Domains
  - Sub1
    - Domain1
  - Sub2
    - Domain2
```

And if you can specify base namespace as `Sample.CodeGen.Domains`
```xml
<Exec WorkingDirectory="$(ProjectDir)" 
    Command="dotnet mapster model -a &quot;$(TargetDir)$(ProjectName).dll&quot; -n Sample.CodeGen.Generated -b Sample.CodeGen.Domains" />
```

Code will be generated to
```
Sample.CodeGen
- Generated
  - Sub1
    - Dto1
  - Sub2
    - Dto2
```



### Generate DTOs and mapping codes

There are 3 flavors, to generate DTOs and mapping codes
- [Fluent API](https://github.com/MapsterMapper/Mapster/wiki/Fluent-API-Code-generation): if you don't want to touch your domain classes, or generate DTOs from domain types in different assembly.
- [Attributes](https://github.com/MapsterMapper/Mapster/wiki/Attribute-base-Code-generation): if you would like to keep mapping declaration closed to your domain classes.
- [Interfaces](https://github.com/MapsterMapper/Mapster/wiki/Interface-base-Code-generation): if you already have DTOs, and you would like to define mapping through interfaces.

### Sample

- https://github.com/MapsterMapper/Mapster/tree/master/src/Sample.CodeGen