### Extension method

You can simply call `Adapt` method from anywhere.

    var destObject = sourceObject.Adapt<TSource, TDestination>();

or just

    var destObject = sourceObject.Adapt<TDestination>();

You might notice that there are 2 extension methods doing the same thing. In fact, `sourceObject.Adapt<TSource, TDestination>` is a bit better in term of performance (different is just casting from object type). If your application doesn't require high performance, you can just use `sourceObject.Adapt<TDestination>` signature. 

### Builder

In most case `Adapt` method is enough, but sometimes we need builder to support fancy scenario. Basic example, is to pass run-time value.

```
var dto = poco.BuildAdapter()
              .AddParameters("user", this.User.Identity.Name)
              .AdaptToType<SimpleDto>();
```

Or we can see how Mapster generate mapping logic with Debugger plugin (https://github.com/chaowlert/Mapster/wiki/Debugging).

```
var script = poco.BuildAdapter()
                .CreateMapExpression<SimpleDto>()
                .ToScript();
```

Or we can map to EF6 object context (https://github.com/chaowlert/Mapster/wiki/EF6).

```
var poco = dto.BuildAdapter()
              .CreateEntityFromContext(db)
              .AdaptToType<DomainPoco>();
```

### Mapper instance

In some cases, you need an instance of a mapper (or a factory function) to pass into a DI container. Mapster has
the IAdapter and Adapter to fill this need:

    IAdapter adapter = new Adapter();

And usage is the same as with the static methods.

    var result = adapter.Adapt<TDestination>(source);
