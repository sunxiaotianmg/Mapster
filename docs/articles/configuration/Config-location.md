### Entry point

Configuration should be set only once and reuse for mapping. Therefore, we should not keep configuration and mapping in the same location. For example:

```csharp
config.ForType<Poco, Dto>().Ignore("Id");
var dto1 = poco1.Adapt<Dto>(config);

config.ForType<Poco, Dto>().Ignore("Id"); //<--- Exception occurred here, because config was already compiled
var dto2 = poco2.Adapt<Dto>(config);
```

Therefore, you should separate configuration and mapping. Configuration should keep in entry point such as `Main` function or `Global.asax.cs` or `Startup.cs`.

```csharp
// Application_Start in Global.asax.cs
config.ForType<Poco, Dto>().Ignore("Id");
```

```csharp
// in Controller class
var dto1 = poco1.Adapt<Dto>(config);
var dto2 = poco2.Adapt<Dto>(config);
```

### Keep together with mapping

A potential problem with separating configuration and mapping is that the code will be separated into 2 locations. You might remove or alter mapping, and you can forget to update the configuration. `Fork` method allow you to keep config and mapping inline.

```csharp
var dto = poco.Adapt<Dto>(
	config.Fork(forked => forked.ForType<Poco, Dto>().Ignore("Id"));
```

Don't worry about performance, forked config will be compiled only once. When mapping occurs for the second time, `Fork` function will return config from cache.

##### Using Fork in generic class or method

`Fork` method uses filename and line number and the key. But if you use `Fork` method inside generic class or method, you must specify your own key (with all type names) to prevent `Fork` to return invalid config from different type arguments.

```csharp
IQueryable<TDto> GetItems<TPoco, TDto>()
{
    var forked = config.Fork(
        f => f.ForType<TPoco, TDto>().Ignore("Id"), 
        $"MyKey|{typeof(TPoco).FullName}|{typeof(TDto).FullName}");
    return db.Set<TPoco>().ProjectToType<TDto>(forked);
}
```

### In separated assemblies

It's relatively common to have mapping configurations spread across a number of different assemblies.  
Perhaps your domain assembly has some rules to map to domain objects and your web api has some specific rules to map to your api contracts. 

#### Scan method

It can be helpful to allow assemblies to be scanned for these rules so you have some basic method of organizing your rules and not forgetting to have the registration code called. In some cases, it may even be necessary to register the assemblies in a particular order, so that some rules override others. Assembly scanning helps with this.

Assembly scanning is simple, just create any number of `IRegister` implementations in your assembly, then call `Scan` from your `TypeAdapterConfig` class:

```csharp
public class MyRegister : IRegister
{
	public void Register(TypeAdapterConfig config)
	{
		config.NewConfig<TSource, TDestination>();

		//OR to create or enhance an existing configuration
		config.ForType<TSource, TDestination>();
	}
}
```

To scan and register at the Global level:

```csharp
TypeAdapterConfig.GlobalSettings.Scan(assembly1, assembly2, assemblyN)
```

For a specific config instance:

```csharp
var config = new TypeAdapterConfig();
config.Scan(assembly1, assembly2, assemblyN);
```

#### Apply method

If you use other assembly scanning library such as MEF, you can easily apply registration with `Apply` method.

```csharp
var registers = container.GetExports<IRegister>();
config.Apply(registers);
```

`Apply` method also allows you to selectively pick from one or more `IRegister` rather than every `IRegister` in assembly.

```csharp
var register = new MockingRegister();
config.Apply(register);
```

### Attributes

You can also set config together with your POCO classes. For example:

```csharp
[AdaptTo(typeof(StudentDto), PreserveReference = true)]
public class Student { 
    ...
}
```