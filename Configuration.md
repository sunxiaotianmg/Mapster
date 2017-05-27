### Setting per type pair

You can easily create settings for a type mapping by using: `TypeAdapterConfig<TSource, TDestination>.NewConfig()`.
When `NewConfig` is called, any previous configuration for this particular TSource => TDestination mapping is dropped.

    TypeAdapterConfig<TSource, TDestination>
        .NewConfig()
        .Ignore(dest => dest.Age)
        .Map(dest => dest.FullName,
             src => string.Format("{0} {1}", src.FirstName, src.LastName));

As an alternative to `NewConfig`, you can use `ForType` in the same way:

	TypeAdapterConfig<TSource, TDestination>
			.ForType()
			.Ignore(dest => dest.Age)
			.Map(dest => dest.FullName,
				 src => string.Format("{0} {1}", src.FirstName, src.LastName));

`ForType` differs in that it will create a new mapping if one doesn't exist, but if the specified TSource => TDestination
mapping does already exist, it will enhance the existing mapping instead of dropping and replacing it.  

### Global setting

Use global settings to apply policies to all mappings.

    TypeAdapterConfig.GlobalSettings.Default.PreserveReference(true);

Then for individual type mappings, you can easily override the global setting(s).

    TypeAdapterConfig<SimplePoco, SimpleDto>.NewConfig().PreserveReference(false);

### Settings inheritance

Type mappings will automatically inherit for source types. Ie. if you set up following config.

    TypeAdapterConfig<SimplePoco, SimpleDto>.NewConfig()
        .Map(dest => dest.Name, src => src.Name + "_Suffix");

A derived type of `SimplePoco` will automatically apply the base mapping config.

    var dest = TypeAdapter.Adapt<DerivedPoco, SimpleDto>(src); //dest.Name = src.Name + "_Suffix"

If you don't wish a derived type to use the base mapping, you can turn off by `AllowImplicitSourceInheritance`

    TypeAdapterConfig.GlobalSettings.AllowImplicitSourceInheritance = true;

And by default, Mapster will not inherit destination type mappings. You can turn on by `AllowImplicitDestinationInheritance`.

    TypeAdapterConfig.GlobalSettings.AllowImplicitDestinationInheritance = true;

Finally, Mapster also provides methods to inherit explicitly.

    TypeAdapterConfig<DerivedPoco, DerivedDto>.NewConfig()
        .Inherits<SimplePoco, SimpleDto>();

### Rule based settings

To set the setting at a more granular level. You can use the `When` method in global settings.
In the example below, when any source type and destination type are the same, we will not the copy the `Id` property.

    TypeAdapterConfig.GlobalSettings.When((srcType, destType, mapType) => srcType == destType)
        .Ignore("Id");

In this example, the config would only apply to Query Expressions (projections).

    TypeAdapterConfig.GlobalSettings.When((srcType, destType, mapType) => mapType == MapType.Projection)
        .IgnoreAttribute(typeof(NotMapAttribute));

### Destination type only

Setting also be able to done without knowing the source type, by using `ForDestinationType`. For example, you can do `AfterMapping` setting to validate after mapping.

    TypeAdapterConfig.GlobalSettings.ForDestinationType<IValidator>()
                     .AfterMapping(dest => dest.Validate());

### Open generics

If mapping type is generic, you can create setting by passing generic type definition to `ForType`.

    TypeAdapterConfig.GlobalSettings.ForType(typeof(GenericPoco<>), typeof(GenericDto<>))
        .Map("value", "Value");

