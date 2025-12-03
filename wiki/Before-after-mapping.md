### Before mapping action

You can perform actions before mapping started by using `BeforeMapping` method.

```csharp
TypeAdapterConfig<Foo, Bar>.ForType()
    .BeforeMapping((src, result) => result.Initialize());
```

### After mapping action

You can perform actions after each mapping by using `AfterMapping` method. For instance, you might would like to validate object after each mapping.

```csharp
TypeAdapterConfig<Foo, Bar>.ForType()
    .AfterMapping((src, result) => result.Validate());
```

Or you can set for all mappings to types which implemented a specific interface by using `ForDestinationType` method.

```csharp
TypeAdapterConfig.GlobalSettings.ForDestinationType<IValidatable>()
    .AfterMapping(result => result.Validate());
```
### Before & after mapping in code generation

`BeforeMapping` and `AfterMapping` accept action which allowed you to pass multiple statements. In code generation, you might need to pass expression instead of action using `BeforeMappingInline` and `AfterMappingInline`, expression can be translated into code, but action cannot.

#### Single line statement

For single line statement, you can directly change from `BeforeMapping` and `AfterMapping` to `BeforeMappingInline` and `AfterMappingInline`.

```csharp
TypeAdapterConfig.GlobalSettings.ForDestinationType<IValidatable>()
    .AfterMappingInline(result => result.Validate());
```

#### Multiple statements

For multiple statements, you need to declare a method for actions.

```csharp
public static void Validate(Dto dto) {
    action1(dto);
    action2(dto);
    ...
}
```

Then you can reference the method to `BeforeMappingInline` and `AfterMappingInline`.

```csharp
TypeAdapterConfig.GlobalSettings.ForDestinationType<IValidatable>()
    .AfterMappingInline(result => PocoToDtoMapper.Validate(result));
```

### Before and After mapping have overloads with `destination` parameter

You can use `BeforeMapping` with `destination` to construct final (`result`) object.

```csharp
TypeAdapterConfig<IEnumerable<int>, IEnumerable<int>>.NewConfig()
  .BeforeMapping((src, result, destination) =>
  {
    if (!ReferenceEquals(result, destination) && destination != null && result is ICollection<int> resultCollection)
    {
      foreach (var item in destination)
      {
        resultCollection.Add(item);
      }
  }
});

IEnumerable<int> source = new List<int> { 1, 2, 3, };
IEnumerable<int> destination = new List<int> { 0, };

var result = source.Adapt(destination);

destination.ShouldBe(new List<int> { 0, });
source.ShouldBe(new List<int> { 1, 2, 3, });
result.ShouldBe(new List<int> { 0, 1, 2, 3, });
```

Same with `AfterMapping`.

```csharp
TypeAdapterConfig<SimplePoco, SimpleDto>.NewConfig()
  .ConstructUsing((simplePoco, dto) => new SimpleDto())
  .AfterMapping((src, result, destination) => result.Name += $"{destination.Name}xxx");

var poco = new SimplePoco
{
  Id = Guid.NewGuid(),
  Name = "test",
};

var oldDto = new SimpleDto { Name = "zzz", };
var result = poco.Adapt(oldDto);

result.ShouldNotBeSameAs(oldDto);
result.Id.ShouldBe(poco.Id);
result.Name.ShouldBe(poco.Name + "zzzxxx");
```