To validate your mapping in unit tests and in order to help with "Fail Fast" situations, the following strict mapping modes have been added.

### Explicit Mapping
Forcing all classes to be explicitly mapped:

```csharp
//Default is "false"
TypeAdapterConfig.GlobalSettings.RequireExplicitMapping = true;
//This means you have to have an explicit configuration for each class, even if it's just:
TypeAdapterConfig<Source, Destination>.NewConfig();
```

### Checking Destination Member
Forcing all destination properties to have a corresponding source member or explicit mapping/ignore:

```csharp
//Default is "false"
TypeAdapterConfig.GlobalSettings.RequireDestinationMemberSource = true;
```

### Validating Mappings
Both a specific TypeAdapterConfig<Source, Destination> or all current configurations can be validated. In addition, if Explicit Mappings (above) are enabled, it will also include errors for classes that are not registered at all with the mapper.

```csharp
//Validate a specific config
var config = TypeAdapterConfig<Source, Destination>.NewConfig();
config.Compile();

//Validate globally
TypeAdapterConfig<Source, Destination>.NewConfig();
TypeAdapterConfig<Source2, Destination2>.NewConfig();
TypeAdapterConfig.GlobalSettings.Compile();
```

### Config Compilation
Mapster will automatically compile mapping for first time usage.

```csharp
var result = poco.Adapt<Dto>();
```

However, you can explicitly compile mapping by `Compile` method.

```csharp
//Global config
TypeAdapterConfig.GlobalSettings.Compile();

//Config instance
var config = new TypeAdapterConfig();
config.Compile();
```

Calling `Compile` method on start up helps you validate mapping and detect problem on start-up time, not on run-time.

NOTE: After compile, when you change setting in config, it will generate errors. Therefore, make sure you finish configuration before calling `Compile`. 