# EF 6 & EF Core

### EF 6 & EF Core support

For EF 6

    PM> Install-Package Mapster.EF6

For EF Core

    PM> Install-Package Mapster.EFCore

In EF, objects are tracked, when you copy data from dto to entity containing navigation properties, this plugin will help finding entity object in navigation properties automatically.

#### Mapster.EFCore & EFCore compatiability
- use Mapster.EFCore version 5.x for EFCore 5.x
- use Mapster.EFCore version 3.x for EFCore 3.x
- use Mapster.EFCore version 1.x for EFCore 2.x


#### Usage

Use `EntityFromContext` method to define data context.

```csharp
var poco = db.DomainPoco.Include("Children")
    .Where(item => item.Id == dto.Id).FirstOrDefault();

dto.BuildAdapter()
    .EntityFromContext(db)
    .AdaptTo(poco);
```

Or like this, if you use mapper instance

```csharp
_mapper.From(dto)
    .EntityFromContext(db)
    .AdaptTo(poco);
```

#### EF Core ProjectToType
`Mapster.EFCore` also allows `ProjectToType` from mapper instance.

```csharp
var query = db.Customers.Where(...);
_mapper.From(query)
    .ProjectToType<Dto>();
```