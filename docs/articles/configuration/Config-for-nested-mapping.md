For example if you have parent and child classes.

```csharp
class ParentPoco
{
    public string Id { get; set; }
    public List<ChildPoco> Children { get; set; }
    public string Name { get; set; }
}
class ChildPoco
{
    public string Id { get; set; }
    public List<GrandChildPoco> GrandChildren { get; set; }
}
class GrandChildPoco
{
    public string Id { get; set; }
}
```

And if you have setting on parent type.

```csharp
TypeAdapterConfig<ParentPoco, ParentDto>.NewConfig()
    .PreserveReference(true);
```

By default, children types will not get effect from `PreserveReference`. 

To do so, you must specify all type pairs inside `ParentPoco`.

```csharp
TypeAdapterConfig<ParentPoco, ParentDto>.NewConfig()
    .PreserveReference(true);
TypeAdapterConfig<ChildPoco, ChildDto>.NewConfig()
    .PreserveReference(true);
TypeAdapterConfig<GrandChildPoco, GrandChildDto>.NewConfig()
    .PreserveReference(true);
```

Or you can set `PreserveReference` in global setting.

```csharp
TypeAdapterConfig.GlobalSettings.Default.PreserveReference(true);
```

### Fork

You can use `Fork` command to define config that applies only specified mapping down to nested mapping without polluting global setting.

```csharp
TypeAdapterConfig<ParentPoco, ParentDto>.NewConfig()
    .Fork(config => config.Default.PreserveReference(true));
```

**Ignore if string is null or empty**

Another example, Mapster only can ignore null value ([IgnoreNullValues](https://github.com/MapsterMapper/Mapster/wiki/Shallow-merge#copy-vs-merge)), however, you use `Fork` to ignore null or empty.

```csharp
TypeAdapterConfig<ParentPoco, ParentDto>.NewConfig()
    .Fork(config => config.ForType<string, string>()
        .MapToTargetWith((src, dest) => string.IsNullOrEmpty(src) ? dest : src)
    );
```