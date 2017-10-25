### Primitives

Converting between primitive types (ie. int, bool, double, decimal) is supported, including when those types are nullable. For all other types, if you can cast types in c#, you can also cast in Mapster.

    decimal i = 123.Adapt<decimal>(); //equal to (decimal)123;

### Enums

Mapster maps enums to numerics automatically, but it also maps strings to and from enums automatically in a fast manner.  
The default Enum.ToString() in .Net is quite slow. The implementation in Mapster is double the speed. Likewise, a fast conversion from strings to enums is also included.  If the string is null or empty, the enum will initialize to the first enum value.

In Mapster, flagged enums are also supported.

    var e = "Read, Write, Delete".Adapt<FileShare>();  
    //FileShare.Read | FileShare.Write | FileShare.Delete

For enum to enum with different type, by default, Mapster will map enum by value. You can override to map enum by name by:

    TypeAdapterConfig.GlobalSettings.Default
        .EnumMappingStrategy(EnumMappingStrategy.ByName);

### Strings

When Mapster maps other types to string, Mapster will use `ToString` method. And when ever Mapster maps string to the other types, Mapster will use `Parse` method.

    var s = 123.Adapt<string>(); //equal to 123.ToString();
    var i = "123".Adapt<int>();  //equal to int.Parse("123");

### Collections

This includes mapping among lists, arrays, collections, dictionary including various interfaces: IList<T>, ICollection<T>, IEnumerable<T> etc...

    var list = db.Pocos.ToList();
    var target = list.Adapt<IEnumerable<Dto>>();  

### Mappable Objects

Mapster can map 2 different objects using the following rules
- Source and destination property names are the same. Ex: `dest.Name = src.Name`
- Source has get method. Ex: `dest.Name = src.GetName()`
- Source property has child object which can flatten to destination. Ex: `dest.ContactName = src.Contact.Name` or `dest.Contact_Name = src.Contact.Name`

Example:

    class Staff {
        public string Name { get; set; }
        public int GetAge() { return (DateTime.Now - this.BirthDate).TotalDays / 365.25; }
        public Staff Supervisor { get; set; }
        ...
    }

    struct StaffDto {
        public string Name { get; set; }
        public int Age { get; set; }
        public string SupervisorName { get; set; }
    }

    var dto = staff.Adapt<StaffDto>();  
    //dto.Name = staff.Name, dto.Age = staff.GetAge(), dto.SupervisorName = staff.Supervisor.Name

You can make custom mapping logic by
- Setting custom mapping (https://github.com/MapsterMapper/Mapster/wiki/Custom-mapping)
- Setting custom naming convention (https://github.com/MapsterMapper/Mapster/wiki/Naming-convention)
- Setting by attributes (https://github.com/MapsterMapper/Mapster/wiki/Setting-by-attributes)
- Ignoring members (https://github.com/MapsterMapper/Mapster/wiki/Ignoring-members)
- Setting rule based mapping (https://github.com/MapsterMapper/Mapster/wiki/Rule-based-member-mapping)

Mappable Object types are included:
- POCO classes
- POCO structs
- Dictionary type implement IDictionary<string, T>
- Record types

Example for object to dictionary

```csharp
var point = new { X = 2, Y = 3 };
var dict = src.Adapt<Dictionary<string, int>>();
dict["Y"].ShouldBe(3);
```

Example for record types
	
```csharp
class Person {
    public string Name { get; }
    public int Age { get; }

    public Person(string name, int age) {
        this.Name = name;
        this.Age = age;
    }
}

var src = new { Name = "Mapster", Age = 3 };
var target = src.Adapt<Person>();
``` 

There is limitation on record type mapping. Record type must not have setter and have only one non-empty constructor. And all parameter names must match with properties.
