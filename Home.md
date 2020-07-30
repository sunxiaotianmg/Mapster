# References

### Basic

| Method        | Description           | Link  |
| ------------- |-----------------------| ----- |
| `src.Adapt<Dest>()`     | Mapping to new type | [basic](https://github.com/MapsterMapper/Mapster/wiki/Basic-usages) |
| `src.Adapt(dest)`       | Mapping to existing object      | [basic](https://github.com/MapsterMapper/Mapster/wiki/Basic-usages) |
| `query.ProjectToType<Dest>()` | Mapping from queryable     | [basic](https://github.com/MapsterMapper/Mapster/wiki/Basic-usages) |
| | Convention & Data type support | [data types](https://github.com/MapsterMapper/Mapster/wiki/Data-types) |

#### Mapper instance (for dependency injection)
| Method        | Description           | Link  |
| ------------- |-----------------------| ----- |
| `IMapper mapper = new Mapper()`     | Create mapper instance | [mappers](https://github.com/MapsterMapper/Mapster/wiki/Mappers) |
| `mapper.Map<Dest>(src)`     | Mapping to new type |  |
| `mapper.Map(src, dest)`       | Mapping to existing object      |  |

#### Builder (for complex mapping)
| Method        | Description           | Link  |
| ------------- |-----------------------| ----- |
| `src.BuildAdapter()` <br> `mapper.From(src)`     | Create builder | [mappers](https://github.com/MapsterMapper/Mapster/wiki/Mappers) |
| `.ForkConfig(config => ...)`     | Inline configuration | [config location](https://github.com/MapsterMapper/Mapster/wiki/Config-location) |
| `.AddParameters(name, value)`       | Passing runtime value      | [setting values](https://github.com/MapsterMapper/Mapster/wiki/Setting-values) |
| `.AdaptToType<Dest>()`     | Mapping to new type |  |
| `.AdaptTo(dest)`       | Mapping to existing object      |  |
| `.CreateMapExpression<Dest>()`     | Get mapping expression |  |
| `.CreateMapToTargetExpression<Dest>()`       | Get mapping to existing object expression |  |
| `.CreateProjectionExpression<Dest>()`     | Get mapping from queryable expression |  |

#### Config
| Method        | Description           | Link  |
| ------------- |-----------------------| ----- |
| `TypeAdapterConfig.GlobalSettings` | Global config | [config](https://github.com/MapsterMapper/Mapster/wiki/Configuration) |
| `var config = new TypeAdapterConfig()` | Create new config instance | [config instance](https://github.com/MapsterMapper/Mapster/wiki/Config-instance) |
| `src.Adapt<Dest>(config)` | Passing config to mapping |  |
| `new Mapper(config)` | Passing config to mapper instance | |
| `src.BuildAdapter(config)` | Passing config to builder |  |
| `config.RequireDestinationMemberSource` | Validate all properties are mapped | [config validation](https://github.com/MapsterMapper/Mapster/wiki/Config-validation-&-compilation) |
| `config.RequireExplicitMapping` | Validate all type pairs are defined | [config validation](https://github.com/MapsterMapper/Mapster/wiki/Config-validation-&-compilation) |
| `config.AllowImplicitDestinationInheritance` | Use config from destination based class | [inheritance](https://github.com/MapsterMapper/Mapster/wiki/Config-inheritance) |
| `config.AllowImplicitSourceInheritance` | Use config from source based class | [inheritance](https://github.com/MapsterMapper/Mapster/wiki/Config-inheritance) |
| `config.SelfContainedCodeGeneration` | Generate all nested mapping in 1 method | [code gen](https://github.com/MapsterMapper/Mapster/wiki/CodeGen) |
| `config.Compile()` | Validate mapping instruction & cache | [config validation](https://github.com/MapsterMapper/Mapster/wiki/Config-validation-&-compilation) |
| `config.CompileProjection()` | Validate mapping instruction & cache for queryable |  |
| `config.Clone()` | Copy config | [config instance](https://github.com/MapsterMapper/Mapster/wiki/Config-instance) |
| `config.Fork(forked => ...)` | Inline configuration | [config location](https://github.com/MapsterMapper/Mapster/wiki/Config-location) |

#### Config scanning

| Method        | Description           | Link  |
| ------------- |-----------------------| ----- |
| `IRegister` | Interface for config scanning | [config location](https://github.com/MapsterMapper/Mapster/wiki/Config-location) |
| `config.Scan(...assemblies)` | Scan for config in assemblies | [config location](https://github.com/MapsterMapper/Mapster/wiki/Config-location) |
| `config.Apply(...registers)` | Apply registers directly | [config location](https://github.com/MapsterMapper/Mapster/wiki/Config-location) |

#### Declare settings

| Method        | Description           | Link  |
| ------------- |-----------------------| ----- |
| `config.Default` | Get setting applied to all type pairs | [config](https://github.com/MapsterMapper/Mapster/wiki/Configuration) |
| `TypeAdapterConfig<Src, Dest>.NewConfig()` <br> `config.NewConfig<Src, Dest>()` | Create setting applied to specific type pairs | [config](https://github.com/MapsterMapper/Mapster/wiki/Configuration) |
| `TypeAdapterConfig<Src, Dest>.ForType()` <br> `config.ForType<Src, Dest>()` | Get setting applied to specific type pairs | [config](https://github.com/MapsterMapper/Mapster/wiki/Configuration) |
| `config.ForType(typeof(GenericPoco<>),typeof(GenericDto<>))` | Get setting applied to generic type pairs | [config](https://github.com/MapsterMapper/Mapster/wiki/Configuration) |
| `config.When((src, dest, mapType) => ...)` | Get setting that applied conditionally | [config](https://github.com/MapsterMapper/Mapster/wiki/Configuration) |
| `config.ForDestinationType<Dest>()` | Get setting that applied to specific destination type | [config](https://github.com/MapsterMapper/Mapster/wiki/Configuration) |
|  | Configuration for nested mapping | [nested mapping](https://github.com/MapsterMapper/Mapster/wiki/Config-for-nested-mapping)

#### Settings
| Method        | Description           | Apply to queryable | Link  |
| ------------- |-----------------------| ------------ | ----- |
| `AddDestinationTransform` | Clean up data for a specific type | x | [setting values](https://github.com/MapsterMapper/Mapster/wiki/Setting-values) |
| `AfterMapping` | Add steps after mapping done |  | [before-after](https://github.com/MapsterMapper/Mapster/wiki/Before-after-mapping) |
| `AvoidInlineMapping` | Skip inline process for large type mapping |  | [object reference](https://github.com/MapsterMapper/Mapster/wiki/Object-references) |
| `BeforeMapping` | Add steps before mapping start |  | [before-after](https://github.com/MapsterMapper/Mapster/wiki/Before-after-mapping) |
| `ConstructUsing` | Define how to create object | x | [constructor](https://github.com/MapsterMapper/Mapster/wiki/Constructor-mapping) |
| `EnableNonPublicMembers` | Mapping non-public properties |  | [non-public](https://github.com/MapsterMapper/Mapster/wiki/Mapping-non-public-members) |
| `EnumMappingStrategy` | Choose whether mapping enum by value or by name |  | [data types](https://github.com/MapsterMapper/Mapster/wiki/Data-types) |
| `Fork` | Add new settings without side effect on main config | x | [nested mapping](https://github.com/MapsterMapper/Mapster/wiki/Config-for-nested-mapping) |
| `GetMemberName` | Define how to resolve property name | x | [custom naming](https://github.com/MapsterMapper/Mapster/wiki/Naming-convention) |
| `Ignore` | Ignore specific properties | x | [ignore](https://github.com/MapsterMapper/Mapster/wiki/Ignoring-members) |
| `IgnoreAttribute` | Ignore specific attributes annotated on properties | x | [attribute](https://github.com/MapsterMapper/Mapster/wiki/Setting-by-attributes) |
| `IgnoreIf` | Ignore conditionally | x | [ignore](https://github.com/MapsterMapper/Mapster/wiki/Ignoring-members) |
| `IgnoreMember` | Setup rules to ignore | x | [rule based](https://github.com/MapsterMapper/Mapster/wiki/Rule-based-member-mapping) |
| `IgnoreNonMapped` | Ignore all properties not defined in `Map` | x | [ignore](https://github.com/MapsterMapper/Mapster/wiki/Ignoring-members) |
| `IgnoreNullValues` | Not map if src property is null |  | [shallow & merge](https://github.com/MapsterMapper/Mapster/wiki/Shallow-merge) |
| `Include` | Include derived types on mapping |  | [inheritance](https://github.com/MapsterMapper/Mapster/wiki/Config-inheritance) |
| `IncludeAttribute` | Include specific attributes annotated on properties | x | [attribute](https://github.com/MapsterMapper/Mapster/wiki/Setting-by-attributes) |
| `IncludeMember` | Setup rules to include | x | [rule based](https://github.com/MapsterMapper/Mapster/wiki/Rule-based-member-mapping) |
| `Inherits` | Copy setting from based type | x | [inheritance](https://github.com/MapsterMapper/Mapster/wiki/Config-inheritance) |
| `Map` | Define property pairs | x | [custom mapping](https://github.com/MapsterMapper/Mapster/wiki/Custom-mapping) |
| `MapToConstructor` | Mapping to constructor | x | [constructor](https://github.com/MapsterMapper/Mapster/wiki/Constructor-mapping) |
| `MapToTargetWith` | Define how to map to existing object between type pair |  | [custom conversion](https://github.com/MapsterMapper/Mapster/wiki/Custom-conversion-logic) |
| `MapWith` | Define how to map between type pair | x | [custom conversion](https://github.com/MapsterMapper/Mapster/wiki/Custom-conversion-logic) |
| `MaxDepth` | Limit depth of nested mapping | x | [object reference](https://github.com/MapsterMapper/Mapster/wiki/Object-references) |
| `NameMatchingStrategy` | Define how to resolve property's name | x | [custom naming](https://github.com/MapsterMapper/Mapster/wiki/Naming-convention) |
| `PreserveReference` | Tracking reference when mapping |  | [object reference](https://github.com/MapsterMapper/Mapster/wiki/Object-references) |
| `ShallowCopyForSameType` | Direct assign rather than deep clone if type pairs are the same |  | [shallow & merge](https://github.com/MapsterMapper/Mapster/wiki/Shallow-merge) |
| `TwoWays` | Define type mapping are 2 ways | x | [2-ways & unflattening](https://github.com/MapsterMapper/Mapster/wiki/Two-ways) |
| `Unflattening` | Allow unflatten mapping | x |[2-ways & unflattening](https://github.com/MapsterMapper/Mapster/wiki/Two-ways) |
| `UseDestinationValue` | Use existing property object to map data |  |[readonly-prop](https://github.com/MapsterMapper/Mapster/wiki/Mapping-readonly-prop) |

#### Attributes

| Annotation        | Description           | Link  |
| ------------- |-----------------------| ----- |
| `[AdaptMember(name)]` | Mapping property to different name | [attribute](https://github.com/MapsterMapper/Mapster/wiki/Setting-by-attributes) |
| `[AdaptIgnore(side)]` | Ignore property from mapping | [attribute](https://github.com/MapsterMapper/Mapster/wiki/Setting-by-attributes) |
| `[UseDestinationValue]` | Use existing property object to map data | [attribute](https://github.com/MapsterMapper/Mapster/wiki/Setting-by-attributes) |

#### Plugins

| Plugin | Method        | Description           |
| ------ | ------------- |-----------------------|
| [Async](https://github.com/MapsterMapper/Mapster/wiki/Async) | `setting.AfterMappingAsync` <br> `builder.AdaptToTypeAsync` | perform async operation on mapping |
| [Codegen](https://github.com/MapsterMapper/Mapster/wiki/CodeGen) | `builder.CreateMapExpression<DTO>().ToScript()` | generate mapping code |
| [Debugging](https://github.com/MapsterMapper/Mapster/wiki/Debugging) | `config.Compiler = exp => exp.CompileWithDebugInfo()` | compile to allow step into debugging |
| [Dependency Injection](https://github.com/MapsterMapper/Mapster/wiki/Dependency-Injection) | `MapContext.Current.GetService<IService>()` | Inject service into mapping logic |
| [EF 6 & EF Core](https://github.com/MapsterMapper/Mapster/wiki/EF-6-&-EF-Core) | `builder.EntityFromContext` | Copy data to tracked EF entity |
| [FEC](https://github.com/MapsterMapper/Mapster/wiki/FastExpressionCompiler) | `config.Compiler = exp => exp.CompileFast()` | compile using FastExpressionCompiler |
| [Immutable](https://github.com/MapsterMapper/Mapster/wiki/Immutable) | `config.EnableImmutableMapping()` | mapping to immutable collection |
| [Json.net](https://github.com/MapsterMapper/Mapster/wiki/Json.net) | `config.EnableJsonMapping()` | map json from/to poco and string |