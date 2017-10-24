### Computed value

You can use `Map` method to specify logic to compute value. For example, compute full name from first name and last name.

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
                            .Map(dest => dest.FullName, src => src.FirstName + " " + src.LastName);
```

### Transform value

While `Map` method specify logic for single property, `AddDestinationTransform` allows transforms for all items of a type, such as trimming all strings. But really any operation can be performed on the destination value before assignment.
```csharp
    //Global
    TypeAdapterConfig.GlobalSettings.Default.AddDestinationTransform((string x) => x.Trim());

    //Per mapping configuration
    TypeAdapterConfig<TSource, TDestination>.NewConfig()
        .AddDestinationTransforms((string x) => x.Trim());
```
### Passing run-time value

In some cases, you might would like to pass runtime values (ie, current user). On configuration, we can receive run-time value by `MapContext.Current.Parameters`.

```csharp
TypeAdapterConfig<Poco, Dto>.NewConfig()
                            .Map(dest => dest.CreatedBy,
                                 src => MapContext.Current.Parameters["user"]);
```

To pass run-time value, we need to use `BuildAdapter` method, and call `AddParameters` method to add each parameter.

```csharp
var dto = poco.BuildAdapter()
              .AddParameters("user", this.User.Identity.Name)
              .AdaptToType<Dto>();
```