# 映射配置验证和编译

为了在单元测试中验证映射配置并解决“快速失败”的情况，添加了以下严格的映射模式。 

### 显式映射
通过修改 配置实例 `RequireExplicitMapping` 值决定是否开启强制显示映射。

当 `RequireExplicitMapping` 值为 `true` 时，所有类型映射必须显式配置，即使非常简单：

```csharp
// 默认值为: "false"
TypeAdapterConfig.GlobalSettings.RequireExplicitMapping = true;
// 当你必须为每个类有一个显式的配置，即使它只是:
TypeAdapterConfig<Source, Destination>.NewConfig();
```

### 检查目标类型程序
通过修改 配置实例 `RequireDestinationMemberSource` 值决定是否开启强制目标类型成员检查。

当 `RequireDestinationMemberSource` 值为 `true` 时，所有 目标类型的字段 必须与 源类型字段 保持一致或显示的指定映射或忽略成员：

```csharp
// 默认值为: "false"
TypeAdapterConfig.GlobalSettings.RequireDestinationMemberSource = true;
```

### 验证映射配置
调用 `TypeAdapterConfig<Source, Destination>.NewConfg()` 的 `Compile` 方法将验证 特定类型的映射配置是否存在错误；

调用 配置实例 的 `Compile` 方法以验证 配置实例中的映射配置 是否存在错误；

另外，如果启用了 显式映射 , 它还将包含没有在映射器中注册的类的错误。

```csharp
// 验证特定配置
var config = TypeAdapterConfig<Source, Destination>.NewConfig();
config.Compile();

// 验证整个配置实例的配置
TypeAdapterConfig<Source, Destination>.NewConfig();
TypeAdapterConfig<Source2, Destination2>.NewConfig();
TypeAdapterConfig.GlobalSettings.Compile();
```

### 配置编译
Mapster 将在第一次调用映射时自动编译：

```csharp
var result = poco.Adapt<Dto>();
```

你也可以通过调用 配置实例 或 特定映射配置的`Compile` 方法编译映射：

```csharp
// 全局配置实例
TypeAdapterConfig.GlobalSettings.Compile();

// 配置实例
var config = new TypeAdapterConfig();
config.Compile();

// 特定配置
var config = TypeAdapterConfig<Source, Destination>.NewConfig();
config.Compile();
```

推荐在程序添加映射配置完成后调用一次 `Compile` 方法，可以快速验证 映射配置中是否存在错误，而不是在运行到某一行业务代码时触发错误降低效率。

> 注意！调用 `Compile` 方法前应该完成所有的映射配置，调用 `Compile` 方法之后 配置实例 就不允许添加修改其它映射配置！

