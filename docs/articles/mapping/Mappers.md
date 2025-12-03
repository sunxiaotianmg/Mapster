### Extension method

You can simply call `Adapt` method from anywhere.

```csharp
var dest = src.Adapt<TSource, TDestination>();
```

or just

```csharp
var dest = src.Adapt<TDestination>();
```

2 extension methods are doing the same thing. `src.Adapt<TDestination>` will cast `src` to object. Therefore, if you map value type, please use `src.Adapt<TSource, TDestination>` to avoid boxing and unboxing.

### Mapper instance

In some cases, you need an instance of a mapper (or a factory function) to pass into a DI container. Mapster has
the `IMapper` and `Mapper` to fill this need:

```csharp
IMapper mapper = new Mapper();
```

And usage `Map` method to perform mapping.

```csharp
var result = mapper.Map<TDestination>(source);
```

### Builder

In most case `Adapt` method is enough, but sometimes we need builder to support fancy scenario. Basic example, is to pass run-time value.

```csharp
var dto = poco.BuildAdapter()
              .AddParameters("user", this.User.Identity.Name)
              .AdaptToType<SimpleDto>();
```

Or if you use mapper instance, you can create builder by method `From`.

```csharp
var dto = mapper.From(poco)
              .AddParameters("user", this.User.Identity.Name)
              .AdaptToType<SimpleDto>();
```

### Code generation

See [Mapster.Tool](https://github.com/MapsterMapper/Mapster/wiki/Mapster.Tool) for generating your specific mapper class, rather than using the provided mappers.