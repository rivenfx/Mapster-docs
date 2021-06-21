# Immutable 类型支持

这个插件将支持映射到 Immutable 类型

### 安装nuget包

    PM> Install-Package Mapster.Immutable

### 如何使用

对配置实例调用 `EnableImmutableMapping` 方法启用Immutable类型映射：

**全局配置实例**

```csharp
TypeAdapterConfig.GlobalSettings.EnableImmutableMapping();
```

**手动实例化的配置实例**

```csharp
config.EnableImmutableMapping();
```

启用Immutable插件将支持映射到以下类型：

- `IImmutableDictionary<,>`
- `IImmutableList<>`
- `IImmutableQueue<>`
- `IImmutableSet<>`
- `IImmutableStack<>`
- `ImmutableArray<>`
- `ImmutableDictionary<,>`
- `ImmutableHashSet<>`
- `ImmutableList<>`
- `ImmutableQueue<>`
- `ImmutableSortedDictionary<,>`
- `ImmutableSortedSet<>`
- `ImmutableStack<>`



