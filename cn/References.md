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
| `src.BuildAdapter()` <br> `mapper.From(src)`     | 创建 builder | [映射器](Mappers.md) |
| `.ForkConfig(config => ...)`     | 内联配置                       | [映射配置位置](Config-location.md) |
| `.AddParameters(name, value)`      | 传递动态值 | [设置值](Setting-values.md) |
| `.AdaptToType<Dest>()`     | 映射到新类型 |  |
| `.AdaptTo(dest)`       | 映射到现有对象                     |  |
| `.CreateMapExpression<Dest>()`     | 获取映射表达式树对象 |  |
| `.CreateMapToTargetExpression<Dest>()`       | 获取映射到现有对象的表达式树对象 |  |
| `.CreateProjectionExpression<Dest>()`     | 获取映射 IQueryable 的表达式树对象 |  |

#### 配置
| 方法        | 说明           | 链接  |
| ------------- |-----------------------| ----- |
| `TypeAdapterConfig.GlobalSettings` | 全局配置 | [映射配置](Configuration.md) |
| `var config = new TypeAdapterConfig()` | 创建新的配置实例 | [映射配置实例](Config-instance.md) |
| `src.Adapt<Dest>(config)` | 映射到新类型并指定配置 |  |
| `new Mapper(config)` | 指定映射器的配置 | |
| `src.BuildAdapter(config)` | 指定Builder的配置 |  |
| `config.RequireDestinationMemberSource` | 验证所有属性的映射 | [映射配置验证](Config-validation-&-compilation.md) |
| `config.RequireExplicitMapping` | 是否开启强制显示映射配置 | [映射配置验证](Config-validation-&-compilation.md) |
| `config.AllowImplicitDestinationInheritance` | 使用基于 **目标类** 的配置 | [映射配置继承](Config-inheritance.md) |
| `config.AllowImplicitSourceInheritance` | 使用基于 **源类** 的配置 | [映射配置继承](Config-inheritance.md) |
| `config.SelfContainedCodeGeneration` | 在 **1** 个方法中生成所有嵌套映射 | [TextTemplate](TextTemplate.md) |
| `config.Compile()` | 验证映射配置并缓存映射 | [映射配置验证](Config-validation-&-compilation.md) |
| `config.CompileProjection()` | 验证 IQueryable 映射配置并缓存映射 |  |
| `config.Clone()` | 复制配置 | [映射配置位置](Config-instance.md) |
| `config.Fork(forked => ...)` | 内联配置 | [映射配置位置](Config-location.md) |

#### 扫描配置

| 方法        | 说明         | 链接  |
| ------------- |-----------------------| ----- |
| `IRegister` | 映射配置扫描接口 | [映射配置位置](Config-location.md) |
| `config.Scan(...assemblies)` | 扫描程序集中的映射配置 | [映射配置位置](Config-location.md) |
| `config.Apply(...registers)` | 应用 映射配置 | [映射配置位置](Config-location.md) |

#### 定义映射配置

| 方法        | 说明           | 链接  |
| ------------- |-----------------------| ----- |
| `config.Default` | 配置应用于所有类型映射       | [映射配置](Configuration.md) |
| `TypeAdapterConfig<Src, Dest>.NewConfig()` <br> `config.NewConfig<Src, Dest>()` | 创建应用于特定类型映射的配置 | [映射配置](Configuration.md) |
| `TypeAdapterConfig<Src, Dest>.ForType()` <br> `config.ForType<Src, Dest>()` | 配置应用于特定类型映射 | [映射配置](Configuration.md) |
| `config.ForType(typeof(GenericPoco<>),typeof(GenericDto<>))` | 配置应用于泛型类型映射 | [映射配置](Configuration.md) |
| `config.When((src, dest, mapType) => ...)` | 配置根据条件进行的类型映射 | [映射配置](Configuration.md) |
| `config.ForDestinationType<Dest>()` | 配置应用于特定目标类型 | [映射配置](Configuration.md) |
|   | 嵌套映射的配置 | [嵌套映射](Config-for-nested-mapping.md) |

#### 配置
| 方法        | 说明           | 支持 IQueryable | 链接  |
| ------------- |-----------------------| ------------ | ----- |
| `AddDestinationTransform` | 清除特定类型的数据 | x | [设置值](Setting-values.md) |
| `BeforeMapping` | 在映射开始之前的步骤 |  | [映射前&映射后](Before-after-mapping.md) |
| `AfterMapping` | 映射完成之后的步骤 |  | [映射前&映射后](Before-after-mapping.md) |
| `AvoidInlineMapping` | 对于大型类型映射，跳过内联处理 |  | [对象引用](Object-references.md) |
| `ConstructUsing` | 定义如何创建对象 | x | [构造函数](Constructor-mapping.md) |
| `EnableNonPublicMembers` | 映射非公开成员 |  | [映射非公开成员](Mapping-non-public-members.md) |
| `EnumMappingStrategy` | 选择按值还是按名称映射枚举 |  | [数据类型](Data-types.md) |
| `Fork` | 在主配置上添加没有副作用的新配置 | x | [嵌套映射](Config-for-nested-mapping.md) |
| `GetMemberName` | 定义如何解析成员名 | x | [自定义成员名称](Naming-convention.md) |
| `Ignore` | 忽略特定属性 | x | [映射忽略](Ignoring-members.md) |
| `IgnoreAttribute` | 忽略属性上特定的特性标记 | x | [特性标记](Setting-by-attributes.md) |
| `IgnoreIf` | 忽略表达式 | x | [根据条件映射忽略](Ignoring-members.md) |
| `IgnoreMember` | 忽略设置规则 | x | [基于规则](Rule-based-member-mapping.md) |
| `IgnoreNonMapped` | 忽略 `Map` 中未定义的所有属性 | x | [映射忽略](Ignoring-members.md) |
| `IgnoreNullValues` | 如果 源属性 为空，则不映射 |  | [浅映射和合并映射](Shallow-merge.md) |
| `Include` | 映射包含派生类型 |  | [映射配置继承](Config-inheritance.md) |
| `IncludeAttribute` | 包括在属性上注释的特定的特性标记 | x | [特性标记](Setting-by-attributes.md) |
| `IncludeMember` | 包含的配置规则 | x | [基于规则](Rule-based-member-mapping.md) |
| `Inherits` | 复制基类的映射配置 | x | [映射配置继承](Config-inheritance.md) |
| `Map` | 定义属性映射 | x | [自定义映射](Custom-mapping.md) |
| `MapToConstructor` | 构造函数映射 | x | [构造函数](Constructor-mapping.md) |
| `MapToTargetWith` | 定义如何映射到现有对象 |  | [自定义映射](Custom-conversion-logic.md) |
| `MapWith` | 定义如何映射 | x | [自定义映射](Custom-conversion-logic.md) |
| `MaxDepth` | 定义映射最大深度 | x | [对象引用](Object-references.md) |
| `NameMatchingStrategy` | 定义如何解析属性的名称 | x | [自定义成员名称](Naming-convention.md) |
| `PreserveReference` | 映射时追踪引用 |  | [对象引用](Object-references.md) |
| `ShallowCopyForSameType` | 如果源类型与目标类型相同，直接分配而不是深度克隆 |  | [浅映射和合并映射](Shallow-merge.md) |
| `TwoWays` | 定义双向映射 | x | [双向 & 逆扁平映射](Two-ways.md) |
| `Unflattening` | 逆扁平映射 | x |[双向 & 逆扁平映射](Two-ways.md) |
| `UseDestinationValue` | 使用现有属性对象映射数据 |  |[映射只读属性](Mapping-readonly-prop.md) |

#### Attributes

| 特性        | 说明         | 链接  |
| ------------- |-----------------------| ----- |
| `[AdaptMember(name)]` | 设置属性映射的目标的属性名称 | [基于Attribute的配置](Setting-by-attributes.md) |
| `[AdaptIgnore(side)]`                         | 映射时忽略成员               | [基于Attribute的配置](Setting-by-attributes.md) |
| `[UseDestinationValue]` | 使用现有属性对象映射数据 | [基于Attribute的配置](Setting-by-attributes.md) |
| `[AdaptTo]` `[AdaptFrom]` `[AdaptTwoWays]` | 在POCO类上添加设置 | [映射配置位置](Config-location.md#attributes) |
| `[Mapper]` `[GeneratMapper]` `[PropertyType]` | 为代码生成定义设置 | [Mapster.Tool](Mapster.Tool.md) |

#### 插件库

| 名称 | 方法        | 说明           |
| ------ | ------------- |-----------------------|
| [Async](Async.md) | `setting.AfterMappingAsync` <br> `builder.AdaptToTypeAsync` | 对映射执行异步操作 |
| [Debugging](Debugging.md) | `config.Compiler = exp => exp.CompileWithDebugInfo()` | 编译以允许步进调试 |
| [Dependency Injection](Dependency-Injection.md) | `MapContext.Current.GetService<IService>()` | 依赖注入获取映射服务 |
| [EF 6 & EF Core](EF-6-&-EF-Core.md) | `builder.EntityFromContext` | 将数据复制到跟踪的EF实体 |
| [FEC](FastExpressionCompiler.md) | `config.Compiler = exp => exp.CompileFast()` | 编译使用FastExpressionCompiler |
| [Immutable](Immutable.md) | `config.EnableImmutableMapping()` | 映射到 immutable collection(只读集合.md) |
| [Json.net](Json.net.md) | `config.EnableJsonMapping()` | 映射JSON对象到实体类或字符串；映射实体类或字符串到 JSON 对象 |

#### 代码生成器

| 插件 | 工具        | 说明         |
| ------ | ------------- |-----------------------|
| [Mapster.Tool](Mapster.Tool.md) | `dotnet mapster` | 生成 DTO 并在构建时映射代码 |
| [TextTemplate](TextTemplate.md) | `t4` | 使用 T4 生成映射代码 |
