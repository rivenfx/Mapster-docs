# Json.net

此插件对 Json.net 映射添加支持

### 安装

    PM> Install-Package Mapster.JsonNet

### 如何使用

对配置实例调用 `EnableJsonMapping` 方法启用对 JSON.NET 的映射支持：

**全局配置**

```csharp
TypeAdapterConfig.GlobalSettings.EnableJsonMapping();
```

**手动实例化的配置实例**

```csharp
config.EnableJsonMapping();
```

启用 [JSON.NET](https://www.nuget.org/packages/Mapster.JsonNet/) 插件将支持以下功能：

- 映射 [JSON.NET](https://www.nuget.org/packages/Newtonsoft.Json) 特有的类型，例如将 `JToken`, `JArray`, `JObject` 映射到自定义类型或将自定义类型映射到这些类型。
- 对 [JSON.NET](https://www.nuget.org/packages/Newtonsoft.Json) 类型支持基于字符串的序列化和反序列化。



