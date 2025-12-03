### Custom Destination Object Creation

You can provide a function call to create your destination objects instead of using the default object creation
(which expects an empty constructor). To do so, use the `ConstructUsing` method when configuring.  This method expects
a function that will provide the destination instance. You can call your own constructor, a factory method,
or anything else that provides an object of the expected type.
```csharp
//Example using a non-default constructor
TypeAdapterConfig<TSource, TDestination>.NewConfig()
    .ConstructUsing(src => new TDestination(src.Id, src.Name));

//Example using an object initializer
TypeAdapterConfig<TSource, TDestination>.NewConfig()
    .ConstructUsing(src => new TDestination{Unmapped = "unmapped"});

//Example using an overload with `destination` parameter
TypeAdapterConfig<TSource, TDestination>.NewConfig()
    .ConstructUsing((src, destination) => new TDestination(src.Id, destination?.Name ?? src.Name));
```

### Map to constructor

By default, Mapster will only map to fields and properties. You can configure to map to constructors by `MapToConstructor`.

```csharp
//global level
TypeAdapterConfig.GlobalSettings.Default.MapToConstructor(true);

//type pair
TypeAdapterConfig<Poco, Dto>.NewConfig().MapToConstructor(true);
```

To define custom mapping, you need to use Pascal case.

```csharp
class Poco {
    public string Id { get; set; }
    ...
}
class Dto {
    public Dto(string code, ...) {
        ...
    }
}
```
```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
    .MapToConstructor(true)
    .Map('Code', 'Id'); //use Pascal case
```

If a class has 2 or more constructors, Mapster will automatically select largest number of parameters that satisfy mapping.

```csharp
class Poco {
    public int Foo { get; set; }
    public int Bar { get; set; }
}
class Dto {
    public Dto(int foo) { ... }
    public Dto(int foo, int bar) { ...} //<-- Mapster will use this constructor
    public Dto(int foo, int bar, int baz) { ... }
}
```
Or you can also explicitly pass ConstructorInfo to the method.

```csharp
var ctor = typeof(Dto).GetConstructor(new[] { typeof(int), typeof(int) });
TypeAdapterConfig<Poco, Dto>.NewConfig()
    .MapToConstructor(ctor);
```
