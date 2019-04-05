### AdaptIgnore attribute

When a property decorated with `[AdaptIgnore]`, that property will be excluded from Mapping. For example, if we would like to exclude price to be mapped.

```
public class Product {
    public string Id { get; set; }
    public string Name { get; set; }

    [AdaptIgnore]
    public decimal Price { get; set; }
}
```

### Ignore custom attributes

You can ignore members annotated with any attributes by using the `IgnoreAttribute` method.

    TypeAdapterConfig.GlobalSettings.Default
        .IgnoreAttribute(typeof(JsonIgnoreAttribute));

### AdaptMember attribute
**Map to different name**  
With `AdaptMember` attribute, you can specify name of source or target to be mapped. For example, if we would like to map `Id` to `Code`.

```
public class Product {
    [AdaptMember("Code")]
    public string Id { get; set; }
    public string Name { get; set; }
}
```

**Map to non-public members**  
You can also map non-public members with `AdaptMember` attribute.
```
public class Product {
    [AdaptMember]
    private string HiddenId { get; set; }
    public string Name { get; set; }
}
```

### Rename from custom attributes

You can rename member to be matched by `GetMemberName`. For example, if we would like to rename property based on `JsonProperty` attribute.

    TypeAdapterConfig.GlobalSettings.Default
        .GetMemberName(member => member.GetCustomAttributes(true)
                                       .OfType<JsonPropertyAttribute>()
                                       .FirstOrDefault()?.PropertyName);  //if return null, property will not be renamed

### Include custom attributes

And if we would like to include non-public members decorated with `JsonProperty` attribute, we can do it by `IncludeAttribute`.

    TypeAdapterConfig.GlobalSettings.Default
        .IncludeAttribute(typeof(JsonPropertyAttribute));
