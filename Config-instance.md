### Config instance

You may wish to have different settings in different scenarios.
If you would not like to apply setting at a static level, Mapster also provides setting instance configurations.

    var config = new TypeAdapterConfig();
    config.Default.Ignore("Id");

For instance configurations, you can use the same `NewConfig` and `ForType` methods that are used at the global level with
the same behavior: `NewConfig` drops any existing configuration and `ForType` creates or enhances a configuration.

    config.NewConfig<TSource, TDestination>()
          .Map(dest => dest.FullName,
               src => string.Format("{0} {1}", src.FirstName, src.LastName));

    config.ForType<TSource, TDestination>()
          .Map(dest => dest.FullName,
               src => string.Format("{0} {1}", src.FirstName, src.LastName));

You can apply a specific config instance by passing it to the `Adapt` method. (NOTE: please reuse your config instance to prevent recompilation)

    var result = src.Adapt<TDestination>(config);

Or to an Adapter instance.

    var adapter = new Adapter(config);
    var result = adapter.Adapt<TDestination>(src);

### Clone

If you would like to create configuration instance from existing configuration, you can use `Clone` method. For example, if you would like to clone from global setting.

    var newConfig = TypeAdapterConfig.GlobalSettings.Clone();
    
Or clone from existing configuration instance

    var newConfig = oldConfig.Clone();

### Fork

`Fork` is similar to `Clone`, but `Fork` will allow you to keep configuration and mapping in the same location. See (https://github.com/MapsterMapper/Mapster/wiki/Config-location) for more info.

    var forked = mainConfig.Fork(config => 
        config.ForType<Poco, Dto>()
              .Map(dest => dest.code, src => src.Id));

    var dto = poco.Adapt<Dto>(forked);