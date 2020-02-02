### EF 6 & EF Core support

For EF 6

    PM> Install-Package Mapster.EF6

For EF Core

    PM> Install-Package Mapster.EFCore

In EF, objects are tracked, when you copy data from dto to entity containing navigation properties, this plugin will help finding entity object in navigation properties automatically.

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
