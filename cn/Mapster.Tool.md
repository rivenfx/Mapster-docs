# Mapster.Tool

### 安装 Mapster.Tool
```bash
# 如果已经拥有dotnet-tools.json，则跳过此步骤
dotnet new tool-manifest 

dotnet tool install Mapster.Tool
```

### 安装 Mapster
简单映射的情况下， 只需要安装 `Mapster.Core`:

```
PM> Install-Package Mapster.Core
```

如果需要 `TypeAdapterConfig ` 进行复杂映射配置，需要安装 `Mapster`:

```
PM> Install-Package Mapster
```

### 命令行
Mapster.Tool 提供了3个命令
- **model**: 从实体生成模型
- **extension**: 从实体生成扩展方法
- **mapper**: 从接口生成映射器

并且 Mapster.Tool 提供了以下选项
- -a: 定义输入程序集
- -b: 指定用于生成动态输出和名称空间的基本名称空间
- -n: 定义生成类的命名空间
- -o: 定义输出目录
- -p: 打印完整的类型名称(如果Poco/Dto有相同的名称)
- -r: 生成record 类型而不是POCO类型

### 集成到csproj文件

#### 手动生成
将以下代码添加到 `csproj` 文件中：
```xml
  <Target Name="Mapster">
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet build" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet tool restore" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster model -a &quot;$(TargetDir)$(ProjectName).dll&quot;" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster extension -a &quot;$(TargetDir)$(ProjectName).dll&quot;" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster mapper -a &quot;$(TargetDir)$(ProjectName).dll&quot;" />
  </Target>
```
在 `csproj` 文件目录下生成如下命令:
```bash
dotnet msbuild -t:Mapster
```

#### 在Build时自动生成
将以下代码添加到 `csproj` 文件中：
```xml
  <Target Name="Mapster" AfterTargets="AfterBuild">
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet tool restore" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster model -a &quot;$(TargetDir)$(ProjectName).dll&quot;" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster extension -a &quot;$(TargetDir)$(ProjectName).dll&quot;" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster mapper -a &quot;$(TargetDir)$(ProjectName).dll&quot;" />
  </Target>
```

#### 清理
将以下代码添加到 `csproj` 文件中：
```xml
  <ItemGroup>
    <Generated Include="**\*.g.cs" />
  </ItemGroup>
  <Target Name="CleanGenerated">
    <Delete Files="@(Generated)" />
  </Target>
```
清理命令如下:
```bash
dotnet msbuild -t:CleanGenerated
```

#### 生成完整类型名

如果POCOs和dto有相同的名称，您可能需要使用完整的类型名称来生成，通过 `-p` 选项：
```xml
  <Target Name="Mapster">
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet build" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet tool restore" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster model -a &quot;$(TargetDir)$(ProjectName).dll&quot; -p" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster extension -a &quot;$(TargetDir)$(ProjectName).dll&quot; -p" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster mapper -a &quot;$(TargetDir)$(ProjectName).dll&quot; -p" />
  </Target>
```

#### 动态输出和名称空间
例如，存在以下结构：
```
Sample.CodeGen
- Domains
  - Sub1
    - Domain1
  - Sub2
    - Domain2
```

如果将基本名称空间指定为 `Sample.CodeGen.Domains`:

```xml
<Exec WorkingDirectory="$(ProjectDir)" 
    Command="dotnet mapster model -a &quot;$(TargetDir)$(ProjectName).dll&quot; -n Sample.CodeGen.Generated -b Sample.CodeGen.Domains" />
```

代码将生成到:
```
Sample.CodeGen
- Generated
  - Sub1
    - Dto1
  - Sub2
    - Dto2
```



### 生成Dto和映射代码

有3种方式来生成dto和映射代码
- [Fluent API](Fluent-API-Code-generation.md): 如果不想编辑实体类定义，或者从不同程序集中的实体类型生成dto。
- [Attributes](Attribute-base-Code-generation.md): 如果想保持映射配置到实体类。
- [Interfaces](Interface-base-Code-generation.md): 如果已经有dto，并且想通过接口定义映射。

### Sample

- https://github.com/MapsterMapper/Mapster/tree/master/src/Sample.CodeGen

