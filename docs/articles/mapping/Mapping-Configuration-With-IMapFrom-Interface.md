Before using this feature you have to add this line:
```csharp
TypeAdapterConfig.GlobalSettings.ScanInheritedTypes(Assembly.GetExecutingAssembly());
```
With adding above line to your Startup.cs or Program.cs or any other way to run at startup, you can write mapping configs in the destination class that implements IMapFrom interface

Example:
```csharp
public class InheritedDestinationModel : IMapFrom<SourceModel>
{
    public string Type { get; set; }
    public int Value { get; set; }

    public void ConfigureMapping(TypeAdapterConfig config)
    {
        config.NewConfig<SourceModel, InheritedDestinationModel>()
            .Map(dest => dest.Value, source => int.Parse(source.Value));
    }
}
```

Even if your destination model doesn't have a specific configuration (you don't want to customize anything), you can just inherit from IMapFrom interface

Example:
```csharp
public class DestinationModel : IMapFrom<SourceModel>
{
    public string Type { get; set; }
}
```