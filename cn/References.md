# References

### 基本用法

| 方法      | 说明        | 链接 |
| ------------- |-----------------------| ----- |
| `src.Adapt<Dest>()`     | 映射到新类型 | [基本用法](Basic-usages.md) |
| `src.Adapt(dest)`       | 映射到现有对象 | [基本用法](Basic-usages.md) |
| `query.ProjectToType<Dest>()` | 映射 IQueryable |  |
| | 约定和数据类型支持 | [数据类型](Data-types.md) |

#### Mapper 实例(用于依赖注入)
| 方法    | 说明         | 链接 |
| ------------- |-----------------------| ----- |
| `IMapper mapper = new Mapper()`     | 创建映射器实例 | [映射器](Mappers.md) |
| `mapper.Map<Dest>(src)`     | 映射到新类型 |  |
| `mapper.Map(src, dest)`       | 映射到现有对象 |  |

#### Builder (用于复杂映射)
| 方法      | 说明         | 链接 |
| ------------- |-----------------------| ----- |
| `src.BuildAdapter()` <br> `mapper.From(src)`     | 创建 builder | [mappers](https://github.com/MapsterMapper/Mapster/wiki/Mappers) |
| `.ForkConfig(config => ...)`     | 内连配置                           | [config location](https://github.com/MapsterMapper/Mapster/wiki/Config-location) |
| `.AddParameters(name, value)`       | Passing runtime value      | [setting values](https://github.com/MapsterMapper/Mapster/wiki/Setting-values) |
| `.AdaptToType<Dest>()`     | 映射到新类型 |  |
| `.AdaptTo(dest)`       | 映射到现有对象                     |  |
| `.CreateMapExpression<Dest>()`     | 获取映射表达式树对象 |  |
| `.CreateMapToTargetExpression<Dest>()`       | 获取映射到现有对象的表达式树对象 |  |
| `.CreateProjectionExpression<Dest>()`     | 获取映射 IQueryable 的表达式树对象 |  |

#### Config
| 方法        | 说明           | 链接  |
| ------------- |-----------------------| ----- |
| `TypeAdapterConfig.GlobalSettings` | 全局配置 | [config](https://github.com/MapsterMapper/Mapster/wiki/Configuration) |
| `var config = new TypeAdapterConfig()` | 创建新的配置实例 | [config instance](https://github.com/MapsterMapper/Mapster/wiki/Config-instance) |
| `src.Adapt<Dest>(config)` | 映射到新类型并指定配置 |  |
| `new Mapper(config)` | 指定映射器的配置 | |
| `src.BuildAdapter(config)` | 指定Builder的配置 |  |
| `config.RequireDestinationMemberSource` | 验证所有属性的映射 | [config validation](https://github.com/MapsterMapper/Mapster/wiki/Config-validation-&-compilation) |
| `config.RequireExplicitMapping` | Validate all type pairs are defined | [config validation](https://github.com/MapsterMapper/Mapster/wiki/Config-validation-&-compilation) |
| `config.AllowImplicitDestinationInheritance` | 使用基于 **目标类** 的配置 | [inheritance](https://github.com/MapsterMapper/Mapster/wiki/Config-inheritance) |
| `config.AllowImplicitSourceInheritance` | 使用基于 **源类** 的配置 | [inheritance](https://github.com/MapsterMapper/Mapster/wiki/Config-inheritance) |
| `config.SelfContainedCodeGeneration` | 在 **1** 个方法中生成所有嵌套映射 | [TextTemplate](https://github.com/MapsterMapper/Mapster/wiki/TextTemplate) |
| `config.Compile()` | 验证映射配置并缓存映射 | [config validation](https://github.com/MapsterMapper/Mapster/wiki/Config-validation-&-compilation) |
| `config.CompileProjection()` | 验证 IQueryable 映射配置并缓存映射 |  |
| `config.Clone()` | 复制配置 | [config instance](https://github.com/MapsterMapper/Mapster/wiki/Config-instance) |
| `config.Fork(forked => ...)` | 内联配置 | [config location](https://github.com/MapsterMapper/Mapster/wiki/Config-location) |

#### 扫描配置

| 方法        | 说明         | 链接  |
| ------------- |-----------------------| ----- |
| `IRegister` | 映射配置扫描接口 | [config location](https://github.com/MapsterMapper/Mapster/wiki/Config-location) |
| `config.Scan(...assemblies)` | 扫描程序集中的映射配置 | [config location](https://github.com/MapsterMapper/Mapster/wiki/Config-location) |
| `config.Apply(...registers)` | 应用 映射配置 | [config location](https://github.com/MapsterMapper/Mapster/wiki/Config-location) |

#### Declare settings

| 方法        | 说明           | 链接  |
| ------------- |-----------------------| ----- |
| `config.Default` | 配置应用于所有类型映射       | [config](https://github.com/MapsterMapper/Mapster/wiki/Configuration) |
| `TypeAdapterConfig<Src, Dest>.NewConfig()` <br> `config.NewConfig<Src, Dest>()` | 创建应用于特定类型映射的配置 | [config](https://github.com/MapsterMapper/Mapster/wiki/Configuration) |
| `TypeAdapterConfig<Src, Dest>.ForType()` <br> `config.ForType<Src, Dest>()` | 配置应用于特定类型映射 | [config](https://github.com/MapsterMapper/Mapster/wiki/Configuration) |
| `config.ForType(typeof(GenericPoco<>),typeof(GenericDto<>))` | 配置应用于泛型类型映射 | [config](https://github.com/MapsterMapper/Mapster/wiki/Configuration) |
| `config.When((src, dest, mapType) => ...)` | 配置根据条件进行的类型映射 | [config](https://github.com/MapsterMapper/Mapster/wiki/Configuration) |
| `config.ForDestinationType<Dest>()` | 配置应用于特定目标类型 | [config](https://github.com/MapsterMapper/Mapster/wiki/Configuration) |
|  | Configuration for nested mapping | [nested mapping](https://github.com/MapsterMapper/Mapster/wiki/Config-for-nested-mapping)

#### Settings
| 方法        | 说明           | 支持 IQueryable | 链接  |
| ------------- |-----------------------| ------------ | ----- |
| `AddDestinationTransform` | 清除特定类型的数据 | x | [setting values](https://github.com/MapsterMapper/Mapster/wiki/Setting-values) |
| `BeforeMapping` | 在映射开始之前的步骤 |  | [before-after](https://github.com/MapsterMapper/Mapster/wiki/Before-after-mapping) |
| `AfterMapping` | 映射完成之后的步骤 |  | [before-after](https://github.com/MapsterMapper/Mapster/wiki/Before-after-mapping) |
| `AvoidInlineMapping` | 对于大型类型映射，跳过内联处理 |  | [object reference](https://github.com/MapsterMapper/Mapster/wiki/Object-references) |
| `ConstructUsing` | 定义如何创建对象 | x | [constructor](https://github.com/MapsterMapper/Mapster/wiki/Constructor-mapping) |
| `EnableNonPublicMembers` | 非公开的属性映射 |  | [non-public](https://github.com/MapsterMapper/Mapster/wiki/Mapping-non-public-members) |
| `EnumMappingStrategy` | 选择按值还是按名称映射枚举 |  | [data types](https://github.com/MapsterMapper/Mapster/wiki/Data-types) |
| `Fork` | 在主配置上添加没有副作用的新配置 | x | [nested mapping](https://github.com/MapsterMapper/Mapster/wiki/Config-for-nested-mapping) |
| `GetMemberName` | 定义如何解析属性名 | x | [custom naming](https://github.com/MapsterMapper/Mapster/wiki/Naming-convention) |
| `Ignore` | 忽略特定属性 | x | [ignore](https://github.com/MapsterMapper/Mapster/wiki/Ignoring-members) |
| `IgnoreAttribute` | 忽略属性上特定的特性标记 | x | [attribute](https://github.com/MapsterMapper/Mapster/wiki/Setting-by-attributes) |
| `IgnoreIf` | 忽略表达式 | x | [ignore](https://github.com/MapsterMapper/Mapster/wiki/Ignoring-members) |
| `IgnoreMember` | 忽略设置规则 | x | [rule based](https://github.com/MapsterMapper/Mapster/wiki/Rule-based-member-mapping) |
| `IgnoreNonMapped` | 忽略 `Map` 中未定义的所有属性 | x | [ignore](https://github.com/MapsterMapper/Mapster/wiki/Ignoring-members) |
| `IgnoreNullValues` | 如果 源属性 为空，则不映射 |  | [shallow & merge](https://github.com/MapsterMapper/Mapster/wiki/Shallow-merge) |
| `Include` | 映射包含派生类型 |  | [inheritance](https://github.com/MapsterMapper/Mapster/wiki/Config-inheritance) |
| `IncludeAttribute` | 包括在属性上注释的特定的特性标记 | x | [attribute](https://github.com/MapsterMapper/Mapster/wiki/Setting-by-attributes) |
| `IncludeMember` | 包含的配置规则 | x | [rule based](https://github.com/MapsterMapper/Mapster/wiki/Rule-based-member-mapping) |
| `Inherits` | Copy setting from based type | x | [inheritance](https://github.com/MapsterMapper/Mapster/wiki/Config-inheritance) |
| `Map` | Define property pairs | x | [custom mapping](https://github.com/MapsterMapper/Mapster/wiki/Custom-mapping) |
| `MapToConstructor` | Mapping to constructor | x | [constructor](https://github.com/MapsterMapper/Mapster/wiki/Constructor-mapping) |
| `MapToTargetWith` | Define how to map to existing object between type pair |  | [custom conversion](https://github.com/MapsterMapper/Mapster/wiki/Custom-conversion-logic) |
| `MapWith` | Define how to map between type pair | x | [custom conversion](https://github.com/MapsterMapper/Mapster/wiki/Custom-conversion-logic) |
| `MaxDepth` | Limit depth of nested mapping | x | [object reference](https://github.com/MapsterMapper/Mapster/wiki/Object-references) |
| `NameMatchingStrategy` | Define how to resolve property's name | x | [custom naming](https://github.com/MapsterMapper/Mapster/wiki/Naming-convention) |
| `PreserveReference` | Tracking reference when mapping |  | [object reference](https://github.com/MapsterMapper/Mapster/wiki/Object-references) |
| `ShallowCopyForSameType` | Direct assign rather than deep clone if type pairs are the same |  | [shallow & merge](https://github.com/MapsterMapper/Mapster/wiki/Shallow-merge) |
| `TwoWays` | Define type mapping are 2 ways | x | [2-ways & unflattening](https://github.com/MapsterMapper/Mapster/wiki/Two-ways) |
| `Unflattening` | Allow unflatten mapping | x |[2-ways & unflattening](https://github.com/MapsterMapper/Mapster/wiki/Two-ways) |
| `UseDestinationValue` | Use existing property object to map data |  |[readonly-prop](https://github.com/MapsterMapper/Mapster/wiki/Mapping-readonly-prop) |

#### Attributes

| 特性        | 说明         | 链接  |
| ------------- |-----------------------| ----- |
| `[AdaptMember(name)]` | Mapping property to different name | [attribute](https://github.com/MapsterMapper/Mapster/wiki/Setting-by-attributes) |
| `[AdaptIgnore(side)]` | Ignore property from mapping | [attribute](https://github.com/MapsterMapper/Mapster/wiki/Setting-by-attributes) |
| `[UseDestinationValue]` | Use existing property object to map data | [attribute](https://github.com/MapsterMapper/Mapster/wiki/Setting-by-attributes) |
| `[AdaptTo]` `[AdaptFrom]` `[AdaptTwoWays]` | Add setting on POCO class | [location](https://github.com/MapsterMapper/Mapster/wiki/Config-location#attributes) |
| `[Mapper]` `[GeneratMapper]` `[PropertyType]` | Define setting for code generation | [Mapster.Tool](https://github.com/MapsterMapper/Mapster/wiki/Mapster.Tool) |

#### 插件库

| 名称 | 方法        | 说明           |
| ------ | ------------- |-----------------------|
| [Async](https://github.com/MapsterMapper/Mapster/wiki/Async) | `setting.AfterMappingAsync` <br> `builder.AdaptToTypeAsync` | perform async operation on mapping |
| [Debugging](https://github.com/MapsterMapper/Mapster/wiki/Debugging) | `config.Compiler = exp => exp.CompileWithDebugInfo()` | compile to allow step into debugging |
| [Dependency Injection](https://github.com/MapsterMapper/Mapster/wiki/Dependency-Injection) | `MapContext.Current.GetService<IService>()` | Inject service into mapping logic |
| [EF 6 & EF Core](https://github.com/MapsterMapper/Mapster/wiki/EF-6-&-EF-Core) | `builder.EntityFromContext` | Copy data to tracked EF entity |
| [FEC](https://github.com/MapsterMapper/Mapster/wiki/FastExpressionCompiler) | `config.Compiler = exp => exp.CompileFast()` | compile using FastExpressionCompiler |
| [Immutable](https://github.com/MapsterMapper/Mapster/wiki/Immutable) | `config.EnableImmutableMapping()` | mapping to immutable collection |
| [Json.net](https://github.com/MapsterMapper/Mapster/wiki/Json.net) | `config.EnableJsonMapping()` | map json from/to poco and string |

#### 代码生成器

| 插件 | 工具        | 说明         |
| ------ | ------------- |-----------------------|
| [Mapster.Tool](https://github.com/MapsterMapper/Mapster/wiki/Mapster.Tool) | `dotnet mapster` | 生成 DTO 并在构建时映射代码 |
| [TextTemplate](https://github.com/MapsterMapper/Mapster/wiki/TextTemplate) | `t4` | 使用 T4 生成映射代码 |
