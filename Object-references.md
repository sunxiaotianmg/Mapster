### Preserve reference (preventing circular reference stackoverflow)

When mapping objects with circular references, a stackoverflow exception will result. This is because Mapster will get stuck in a loop trying to recursively map the circular reference. If you would like to map circular references or preserve references (such as 2 properties pointing to the same object), you can do it by setting `PreserveReference` to `true`

    TypeAdapterConfig<TSource, TDestination>
        .NewConfig()
        .PreserveReference(true);

NOTE: Projection doesn't support circular reference. To overcome, you might use `Adapt` instead of `ProjectToType`.

    TypeAdaptConfig.GlobalSettings.Default.PreserveReference(true);
    var students = context.Student.Include(p => p.Schools).Adapt<List<StudentDTO>>();

NOTE: in Mapster setting is per type pair, not per hierarchy (see https://github.com/MapsterMapper/Mapster/wiki/Config-for-nested-classes). Therefore, you need to apply config to all type pairs.

### Shallow copy

By default, Mapster will recursively map nested objects. You can do shallow copying by setting `ShallowCopyForSameType` to `true`.

    TypeAdapterConfig<TSource, TDestination>
        .NewConfig()
        .ShallowCopyForSameType(true);

### Mapping very large objects

For performance optimization, Mapster tried to inline class mapping. This process will takes time if your models are complex.

![](https://cloud.githubusercontent.com/assets/21364231/25666644/ce38c8c0-3029-11e7-8793-8a51c519c2a0.png)

You can skip inlining process by:

    TypeAdapterConfig.GlobalSettings.Default
        .AvoidInlineMapping(true);

