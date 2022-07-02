# StaticDictionaries 📚
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![CodeFactor](https://www.codefactor.io/repository/github/mmaximus111/staticdictionaries/badge)](https://www.codefactor.io/repository/github/mmaximus111/staticdictionaries)
[![NuGet version (SoftCircuits.Silk)](https://img.shields.io/nuget/v/StaticDictionaries?color=blue&style=plastic?logoHeight=45)](https://www.nuget.org/packages/StaticDictionaries)
![example workflow](https://github.com/MMaximus111/StaticDictionaries/.github/workflows/dotnet.yml/badge.svg)

Simple high performance solution for data hardcoding.
Smart. Flexible. Powerful.

Powered by source generators. Without using reflection.

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

## Powerful `enum` management 🦾

Source generator creates some methods in [EnumName]Extensions class for convenient and fast `enum` management:

* `Length` quantity of all enum members.
* `MinId` returns min `Id()`.
* `MaxId` returns max `Id()`.
* `All()` returns an array of all `enum` members.
* `GetById()` returns member or `NotSupportedException`.

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
    [Value("In progress", true)]
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

* `StaticDictionary` parameter names must not be duplicated.
* `StaticDictionary` parameter names must use the English alphabet only because they will be generated into methods.
* All `Value` attrubutes must contain such arguments quantity as `StaticDictionary`.
* All `StaticDictionary` `enum` members must contain `Value` attribute with arguments. 
* Generator creates two methods by default: `Id()` with `(int)member` and `Name()` with `nameof(member)` if you do not override them.
* `enum` members without `Value` attribute will be ignored.
* Parameter types are determined automatically, so all parameters in a sequence must be of the same type.
For example, all of the first types should be `string`, and all of the second types should be `bool`.

## Disclaimer ❗️
The material embodied in this software is provided to you "as-is" and without warranty of any kind, express, implied or otherwise, including without limitation, any warranty of fitness for a particular purpose. In no event shall the Centers for Disease Control and Prevention (CDC) or the United States (U.S.) government be liable to you or anyone else for any direct, special, incidental, indirect or consequential damages of any kind, or any damages whatsoever, including without limitation, loss of profit, loss of use, savings or revenue, or the claims of third parties, whether or not CDC or the U.S. government has been advised of the possibility of such loss, however caused and on any theory of liability, arising out of or in connection with the possession, use or performance of this software.
