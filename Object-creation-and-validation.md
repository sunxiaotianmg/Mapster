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
```
### After mapping action

You can perform actions after each mapping by using `AfterMapping` method. For instance, you might would like to validate object after each mapping.

```csharp
    TypeAdapterConfig<Foo, Bar>.ForType().AfterMapping((src, dest) => dest.Validate());
```

Or you can set for all mappings to types which implemented a specific interface by using `ForDestinationType` method.

```csharp
    TypeAdapterConfig.GlobalSettings.ForDestinationType<IValidatable>()
                                .AfterMapping(dest => dest.Validate());
```
