# Json.net

# Mapster.JsonNet
Json.net conversion supports

### Install

    PM> Install-Package Mapster.JsonNet

### Usage

Call `EnableJsonMapping` from your `TypeAdapterConfig` to enable Json.Net mapping.

```csharp
TypeAdapterConfig.GlobalSettings.EnableJsonMapping();
```

or 

```csharp
config.EnableJsonMapping();
```

This will allow

- Mapping between Json.Net types (`JToken`, `JArray`, `JObject`) from/to POCO types
- Serialize and deserialize Json.Net types from/to string