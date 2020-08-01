### Mapster.Tool

Here are steps to add code generation.

1. Define interface to generate code. Your interface must annotate with `[Mapper]` in order for tool to pickup for generation.

This is example interface.
```csharp
[Mapper]
public interface IProductMapper
{
    ProductDTO Map(Product customer);
}
```

You can add multiple members as you want. All member names are flexible.
```csharp
[Mapper]
public interface ICustomerMapper
{
    //for queryable
    Expression<Func<Customer, CustomerDTO>> ProjectToDto { get; }
    
    //map from POCO to DTO
    CustomerDTO MapToDto(Customer customer);

    //map back from DTO to POCO
    Customer MapToPoco(CustomerDTO dto);

    //map to existing object
    Customer MapToExisting(CustomerDTO dto, Customer customer);
}
```

If you have configuration, it must be in `IRegister`

```csharp
public class MyRegister : IRegister
{
    public void Register(TypeAdapterConfig config)
    {
        config.NewConfig<TSource, TDestination>();
    }
}
```

At this point, you can use your interface in your code to perform mapping.

```csharp
var dtos = context.Customers.Select(mapper.ProjectToDto);
var dto = mapper.MapToDto(poco);
mapper.MapToExisting(dto, poco);
```

2. Install `Mapster.Tool`

```bash
#skip this step if you already have dotnet-tools.json
dotnet new tool-manifest 

dotnet tool install Mapster.Tool
```

NOTE: the tool required .NET Core 2.1 or .NET Core 3.1 on your machine.

3. Add setting to you `csproj`

```xml
<Target Name="mapster" AfterTargets="AfterBuild">
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet tool restore" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster -a $(TargetDir)$(ProjectName).dll" />
</Target>
```

That's it, now Mapster will automatically generate codes after build.

#### Example

You can find example in Benchmark project
- https://github.com/MapsterMapper/Mapster/tree/master/src/Benchmark/Mappers