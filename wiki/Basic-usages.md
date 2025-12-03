### Mapping to a new object
Mapster creates the destination object and maps values to it.

```csharp
var destObject = sourceObject.Adapt<Destination>();
```

### Mapping to an existing object

You make the object, Mapster maps to the object.

```csharp
sourceObject.Adapt(destObject);
```

### Queryable Extensions

Mapster also provides extensions to map queryables.

> [!IMPORTANT]  
> Avoid calling ProjectToType() before materializing queries from Entity Framework. This is known to cause issues. Instead, call ToList() or ToListAsync() before calling ProjectToType.

```csharp
using (MyDbContext context = new MyDbContext())
{
    // Build a Select Expression from DTO
    var destinations = context.Sources.ProjectToType<Destination>().ToList();

    // Versus creating by hand:
    var destinations = context.Sources.Select(c => new Destination {
        Id = p.Id,
        Name = p.Name,
        Surname = p.Surname,
        ....
    })
    .ToList();
}
```