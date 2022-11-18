# StaticDictionaries 📚
[![.NET](https://github.com/MMaximus111/StaticDictionaries/actions/workflows/dotnet.yml/badge.svg?branch=master)](https://github.com/MMaximus111/StaticDictionaries/actions/workflows/dotnet.yml)
[![CodeFactor](https://www.codefactor.io/repository/github/mmaximus111/staticdictionaries/badge)](https://www.codefactor.io/repository/github/mmaximus111/staticdictionaries)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=MMaximus111_StaticDictionaries&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=MMaximus111_StaticDictionaries)
[![codecov](https://codecov.io/gh/MMaximus111/StaticDictionaries/branch/master/graph/badge.svg?token=FAIIC9CRXP)](https://codecov.io/gh/MMaximus111/StaticDictionaries)
[![License: MIT](https://img.shields.io/badge/License-MIT-purple.svg)](https://opensource.org/licenses/MIT)
[![NuGet version (SoftCircuits.Silk)](https://img.shields.io/nuget/v/StaticDictionaries?color=blue&style=plastic?logoHeight=45)](https://www.nuget.org/packages/StaticDictionaries)


Simple high performance solution for data hardcoding.
Smart. Flexible. Powerful.

Powered by source generators. Without using reflection. `.NET 7 now supported ⚒`

StaticDictionaries is a convinient library for data hardcoding in your project.
Just decorate the `enum` using attributes, the source generator will do the rest for you.
Minimum code and time spent - maximum result and performance. Property management has never been so convenient.
Generated methods for managing `enum` without reflection will make development pleasant and faster than ever.

## Installing StaticDictionaries

StaticDictionaries is available on [NuGet](https://www.nuget.org/packages/StaticDictionaries).

```
dotnet add package StaticDictionaries

Install-Package StaticDictionaries
```
## Basic usage by steps🛸
1. Decorate your enum with `[StaticDictionary]` attribute and define arguments to generate properties:
```csharp
[StaticDictionary("Surname", "Age")]
public enum User
```

2. Decorate your enum members with `[Value]` attribute and define member values:
```csharp
[Value("Brown", 27)]
John = 1
```

3. Build project to run source generator and get access to generated methods:
```csharp
int id = User.John.Id();
int age = User.John.Age();
string name = User.John.Name();
string surname = User.John.Surname();
```

## Powerful `enum` management 🦾

Source generator creates some methods in `[EnumName]Extensions` class for convenient and fast `enum` management:

* `Length` quantity of all enum members.
* `MinId` returns min `Id()`.
* `MaxId` returns max `Id()`.
* `All()` returns an array of all `enum` members.
* `GetById()` returns member or `NotSupportedException`.
* `Json()` returns serialized `enum` in json format.
* `Xml()` returns serialized `enum` in xml format.

## Dictionary supported primitive types 🗿

|  CLR type  | Status | 
|------------|-------|
| `int`   |✅|
| `bool`   |✅|
| `char`   |✅|
| `double`   |✅|
| `string`   |✅|

## Samples 🤝

Employee dictionary with `Name`, `Age` and `Active` properties:
```csharp
[StaticDictionary("Name", "Age", "Active")]
public enum Employee
{
    [Value("Maxim Jhonson", 18, true)]
    Maxim = 1,
    [Value("John Jhonson", 23, false)]
    John = 2
}

public static void Main()
{
    int johnAge = Employee.John.Age();
    int maximAge = Employee.Maxim.Age();
    
    string johnName = Employee.John.Name();
    string maximName = Employee.Maxim.Name();
}

```
Source generator creates [EnumName]Extensions class that provide some powerful functionality for managing your `enum`:
```csharp
[StaticDictionary("Name", "IsLast")]
public enum Status
{
    [Value("New state", false)]
    New = 1,
    [Value("In progress", false)]
    InProgress = 2,
    [Value("Completed", true)]
    Completed = 3
}

public static void Main()
{
    Status status = StatusExtensions.GetById(2);
    
    Status[] statuses = StatusExtensions
                      .All()
                      .Where(x => !x.IsLast())
                      .ToArray();
}

```
`Id()` and `Name()` methods are generated by default if you do not override them.
```csharp
[StaticDictionary]
public enum Metal
{
    [Value]
    Gold = 1,
    [Value]
    Ferrum = 2
}

public static void Main()
{
    int goldId = Metal.Gold.Id(); 
    string ferrumName = Metal.Ferrum.Name();
}

```

## Important notes ⚠️

* `[StaticDictionary]` parameter names must not be duplicated.
* `[StaticDictionary]` parameter names must use the English alphabet only because they will be generated into methods.
* All `[Value]` attrubutes must contain such arguments quantity as `[StaticDictionary]`.
* All `[StaticDictionary]` `enum` members can contain `[Value]` attribute with arguments. 
* Generator creates two methods by default: `Id()` with `(int)member` and `Name()` with `nameof(member)` if you do not override them.
* `enum` members without `[Value]` attribute will be ignored.
* `enum` property names must not be equal to [EnumName]Extensions (c# naming rules for methods).
* `Json()` and `Xml()` methods generated only if you mark `enum` with `[JsonSupport]` or `[XmlSupport]`. 
* `enum` members that use bitwise operators must not contain [Value] attribute.
* Parameter types are determined automatically, so all parameters in a sequence must be of the same type.
For example, all of the first types should be `string`, and all of the second types should be `bool`.

## Performance 🚀
Despite the convenient format of use, the performance remained at a high level, and thanks to separate methods for working with an `Enum` without reflection, performance increased significantly in some indicators:
``` ini
BenchmarkDotNet=v0.13.1, OS=Windows 10.0.19044.1766 (21H2)
AMD Ryzen 5 3600, 1 CPU, 12 logical and 6 physical cores
.NET SDK=6.0.301
  [Host]     : .NET 6.0.6 (6.0.622.26707), X64 RyuJIT  [AttachedDebugger]
  DefaultJob : .NET 6.0.6 (6.0.622.26707), X64 RyuJIT
 ```

|                      Method |        Mean |     Error |    StdDev |      Median |
|---------------------------- |------------:|----------:|----------:|------------:|
|   ClassicAccessToMemberName |   0.0001 ns | 0.0002 ns | 0.0001 ns |   0.0001 ns |
| GeneratedAccessToMemberName |   0.0039 ns | 0.0041 ns | 0.0030 ns |   0.0014 ns |
|     ReflectionGetAllMembers | 562.4409 ns | 3.8498 ns | 3.6011 ns | 562.4710 ns |
|      GeneratedGetAllMembers |   5.6408 ns | 0.1011 ns | 0.0946 ns |   5.6575 ns |
|           GetEnumMemberById |   0.0008 ns | 0.0008 ns | 0.0007 ns |   0.0004 ns |
|  GeneratedGetEnumMemberById |   0.0001 ns | 0.0002 ns | 0.0002 ns |   0.0000 ns |

