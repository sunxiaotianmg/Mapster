### Custom member mapping

You can customize how Mapster maps values to a property.

    TypeAdapterConfig<TSource, TDestination>
        .NewConfig()
        .Map(dest => dest.FullName,
             src => string.Format("{0} {1}", src.FirstName, src.LastName));

You can even map when source and destination property types are different.

    TypeAdapterConfig<TSource, TDestination>
        .NewConfig()
        .Map(dest => dest.Gender,      //Genders.Male or Genders.Female
             src => src.GenderString); //"Male" or "Female"

### Mapping with condition

The Map configuration can accept a third parameter that provides a condition based on the source.
If the condition is not met, Mapster will retry with next conditions. Default condition should be added at the end without specifying condition. If you do not specify default condition, null or default value will be assigned.

    TypeAdapterConfig<TSource, TDestination>
        .NewConfig()
        .Map(dest => dest.FullName, src => "Sig. " + src.FullName, srcCond => srcCond.Country == "Italy")
        .Map(dest => dest.FullName, src => "Sr. " + src.FullName, srcCond => srcCond.Country == "Spain")
        .Map(dest => dest.FullName, src => "Mr. " + src.FullName);

NOTE: if you would like to skip mapping, when condition is met, you can use `IgnoreIf` (https://github.com/MapsterMapper/Mapster/wiki/Ignoring-members#ignore-conditionally).

### Mapping to non-public members

`Map` command can map to private member by specify name of the members.

    TypeAdapterConfig<TSource, TDestination>
        .NewConfig()
        .Map("PrivateDestName", "PrivateSrcName");

For more information about mapping non-public members, please see https://github.com/MapsterMapper/Mapster/wiki/Mapping-non-public-members.
