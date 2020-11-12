## Mapster.Tool

```bash
#skip this step if you already have dotnet-tools.json
dotnet new tool-manifest 

dotnet tool install Mapster.Tool
```

### Commands
Mapster.Tool provides 3 commands
- **model**: generate models from entities
- **extension**: generate extension methods from entities
- **mapper**: generate mappers from interfaces

And Mapster.Tool provides following options
- -a: define input assembly
- -n: define namespace of generated classes
- -o: define output directory

### csproj integration

#### Generate manually
add following code to your `csproj` file.
```xml
  <Target Name="Mapster">
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet build" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet tool restore" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster model -a $(TargetDir)$(ProjectName).dll" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster extension -a $(TargetDir)$(ProjectName).dll" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster mapper -a $(TargetDir)$(ProjectName).dll" />
  </Target>
```
to generate run following command on `csproj` file directory:
```bash
dotnet msbuild -t:Mapster
```

#### Generate automatically on build
add following code to your `csproj` file.
```xml
  <Target Name="Mapster" AfterTargets="AfterBuild">
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet tool restore" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster model -a $(TargetDir)$(ProjectName).dll" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster extension -a $(TargetDir)$(ProjectName).dll" />
    <Exec WorkingDirectory="$(ProjectDir)" Command="dotnet mapster mapper -a $(TargetDir)$(ProjectName).dll" />
  </Target>
```

#### Clean up
add following code to your `csproj` file.
```xml
  <ItemGroup>
    <Generated Include="**\*.g.cs" />
  </ItemGroup>
  <Target Name="CleanGenerated">
    <Delete Files="@(Generated)" />
  </Target>
```
to clean up run following command:
```bash
dotnet msbuild -t:CleanGenerated
```

### Generate models

Annotate your class with `[AdaptFrom]`, `[AdaptTo]`, or `[AdaptTwoWays]`.

Example:
```csharp
[AdaptTo("[name]Dto")]
public class Student {
    ...
}
```

Then Mapster will generate:
```csharp
public class StudentDto {
    ...
}
```

#### Ignore some properties on generation

By default, code generation will ignore properties that annotated `[AdaptIgnore]` attribute. But you can add more settings which include `IgnoreAttributes`, `IgnoreNoAttributes`, `IgnoreNamespaces`.

Example:
```csharp
[AdaptTo("[name]Dto", IgnoreNoAttributes = new[] { typeof(DataMemberAttribute) })]
public class Student {

    [DataMember]
    public string Name { get; set; }     //this property will be generated
    
    public string LastName { get; set; } //this will not be generated
}
```

#### Change property types

By default, if property type annotated with the same adapt attribute, code generation will forward to that type. (For example, `Student` has `ICollection<Enrollment>`, after code generation `StudentDto` will has `ICollection<EnrollmentDto>`).

You can override this by `[PropertyType(typeof(Target))]` attribute. This annotation can be annotated to either on property or on class.

For example:
```csharp
[AdaptTo("[name]Dto")]
public class Student {
    public ICollection<Enrollment> Enrollments { get; set; }
}

[AdaptTo("[name]Dto"), PropertyType(typeof(DataItem))]
public class Enrollment {
    [PropertyType(typeof(string))]
    public Grade? Grade { get; set; }
}
```

This will generate:
```csharp
public class StudentDto {
    public ICollection<DataItem> Enrollments { get; set; }
}
public class EnrollmentDto {
    public string Grade { get; set; }
}
```

#### Generate readonly properties

For `[AdaptTo]` and `[AdaptTwoWays]`, you can generate readonly properties with `MapToConstructor` setting.

For example:
```csharp
[AdaptTo("[name]Dto", MapToConstructor = true)]
public class Student {
    public string Name { get; set; }
}
```

This will generate:
```csharp
public class StudentDto {
    public string Name { get; }

    public StudentDto(string name) {
        this.Name = name;
    }
}
```

#### Generate nullable properties

For `[AdaptFrom]`, you can generate nullable properties with `IgnoreNullValues` setting.

For example:
```csharp
[AdaptFrom("[name]Merge", IgnoreNullValues = true)]
public class Student {
    public int Age { get; set; }
}
```

This will generate:
```csharp
public class StudentMerge {
    public int? Age { get; set; }
}
```

### Generate extension methods

#### Generate using `[GenerateMapper]` attribute
For any POCOs annotate with `[AdaptFrom]`, `[AdaptTo]`, or `[AdaptTwoWays]`, you can add `[GenerateMapper]` in order to generate extension methods.

Example:
```csharp
[AdaptTo("[name]Dto"), GenerateMapper]
public class Student {
    ...
}
```

Then Mapster will generate:
```csharp
public class StudentDto {
    ...
}
public static class StudentMapper {
    public static StudentDto AdaptToDto(this Student poco) { ... }
    public static StudentDto AdaptTo(this Student poco, StudentDto dto) { ... }
    public static Expression<Func<Student, StudentDto>> ProjectToDto => ...
}
```

#### Generate using configuration

You can also generate extension methods and add extra settings from configuration.

```csharp
public class MyRegister : IRegister
{
    public void Register(TypeAdapterConfig config)
    {
        config.NewConfig<TSource, TDestination>()
            .GenerateMapper(MapType.Map | MapType.MapToTarget);
    }
}
```


### Generate mapper from interface

Annotate your interface with `[Mapper]` in order for tool to pickup for generation.

This is example interface.
```csharp
[Mapper]
public interface IProductMapper
{
    ProductDTO Map(Product customer);
}
```

You can add multiple members as you want. All member names are flexible, but signature must be in following patterns:
```csharp
[Mapper]
public interface ICustomerMapper
{
    //for queryable
    Expression<Func<Customer, CustomerDTO>> ProjectToDto { get; }
    
    //map from POCO to DTO
    CustomerDTO MapToDto(Customer customer);

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

### Sample

- https://github.com/MapsterMapper/Mapster/tree/master/src/Sample.CodeGen