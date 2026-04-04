MagicMapper.Extensions.EnumMapping
===========
[![CI](https://github.com/NET-Sorcery/MagicMapper.Extensions.EnumMapping/actions/workflows/ci.yml/badge.svg?branch=master)](https://github.com/NET-Sorcery/MagicMapper.Extensions.EnumMapping/actions/workflows/ci.yml)
[![NuGet](https://img.shields.io/nuget/dt/MagicMapper.Extensions.EnumMapping.svg)](https://www.nuget.org/packages/MagicMapper.Extensions.EnumMapping) 
[![NuGet](https://img.shields.io/nuget/vpre/MagicMapper.Extensions.EnumMapping.svg)](https://www.nuget.org/packages/MagicMapper.Extensions.EnumMapping)
[![MyGet (dev)](https://img.shields.io/myget/automapperdev/v/AutoMapper.Extensions.EnumMapping.svg)](http://myget.org/gallery/automapperdev)

### Summary

The MagicMapper.Extensions.EnumMapping library gives you control about your enum values mappings. It is possible to create a custom type converter for every enum.

This library supports mapping enums values like properties.

### Dependencies

- [MagicMapper](https://www.nuget.org/packages/MagicMapper/)

### Installing MagicMapper.Extensions.EnumMapping

You should install [MagicMapper.Extensions.EnumMapping with NuGet](https://www.nuget.org/packages/MagicMapper.Extensions.EnumMapping):

    Install-Package MagicMapper.Extensions.EnumMapping

Or via the .NET Core command line interface:

    dotnet add package MagicMapper.Extensions.EnumMapping

Either commands, from Package Manager Console or .NET Core CLI, will download and install MagicMapper.Extensions.EnumMapping. MagicMapper.Extensions.EnumMapping has no dependencies. 

### Usage
Install via NuGet first:
`Install-Package MagicMapper.Extensions.EnumMapping`

To use it:

For method `CreateMap` this library provide a `ConvertUsingEnumMapping` method. This method add all default mappings from source to destination enum values.

If you want to change some mappings, then you can use `MapValue` method. This is a chainable method.

Default the enum values are mapped by value (`MapByValue()`), but it is possible to map by name calling `MapByName()`. For enums which does not have same values and names, you can use `MapByCustom()`. Then you have to add a `MapValue` for every source enum value.

```csharp
using AutoMapper.Extensions.EnumMapping;

public enum Source
{
    Default = 0,
    First = 1,
    Second = 2
}

public enum Destination
{
    Default = 0,
    Second = 2
}

internal class YourProfile : Profile
{
    public YourProfile()
    {
        CreateMap<Source, Destination>()
            .ConvertUsingEnumMapping(opt => opt
		// optional: .MapByValue() or MapByName(), without configuration MapByValue is used
		.MapValue(Source.First, Destination.Default))
            .ReverseMap(); // to support Destination to Source mapping, including custom mappings of ConvertUsingEnumMapping
    }
}
    ...
```

### Testing

[MagicMapper](https://www.nuget.org/packages/MagicMapper/) provides a nice tooling for validating typemaps. This library adds an extra `EnumMapperConfigurationExpressionExtensions.EnableEnumMappingValidation` extension method to extend the existing `AssertConfigurationIsValid()` method to validate also the enum mappings.

To enable testing the enum mapping configuration:

```csharp

public class MappingConfigurationsTests
{
    [Fact]
    public void WhenProfilesAreConfigured_ItShouldNotThrowException()
    {
        // Arrange
        var config = new MapperConfiguration(configuration =>
        {
            configuration.EnableEnumMappingValidation();

            configuration.AddMaps(typeof(AssemblyInfo).GetTypeInfo().Assembly);
        });
		
        // Assert
        config.AssertConfigurationIsValid();
    }
}
```
