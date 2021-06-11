# Immutable

# Mapster.Immutable
Immutable collection supports

### Install

    PM> Install-Package Mapster.Immutable

### Usage

Call `EnableImmutableMapping` from your `TypeAdapterConfig` to enable Immutable collection.

```csharp
TypeAdapterConfig.GlobalSettings.EnableImmutableMapping();
```

or 

```csharp
config.EnableImmutableMapping();
```

This will allow mapping to 
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