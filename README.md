# StaticDictionaries 📚
Simple high performance solution for data hardcoding.
Smart. Flexible. Powerful.

Powered by source generators. Without using reflection ❌

StaticDictionaries is a convinient library for data hardcoding in your project.
Just describe the `enum` using attributes, the source generator will do the rest for you.
Minimum code and time spent - maximum result and performance. Property management has never been so convenient.
Helper methods for managing `enum` without reflection will make development pleasant and faster than ever.

## Installing StaticDictionaries

StaticDictionaries is available on [NuGet](https://www.nuget.org/packages/StaticDictionaries).

```
dotnet add package StaticDictionaries

Install-Package StaticDictionaries
```

## Dictionary supported primitive types 🗿

|  CLR type  | Status | 
|------------|-------|
| `int`   |✅|
| `bool`   |✅|
| `char`   |✅|
| `double`   |✅|
| `decimal`   |✅|
| `string`   |✅|

## Samples

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

## Possible errors ⚠️

* `StaticDictionary` parameter names must not be duplicated.
* All `Value` attrubutes must contain such parameters quantity as `StaticDictionary`.
* All `StaticDictionary` `enum` members must contain `Value` attribute.
* Parameter types are determined automatically, so all parameters in a sequence must be of the same type.
For example, all first types should be `string`, and all second types should be `bool`.


## Disclaimer ❗️
The material embodied in this software is provided to you "as-is" and without warranty of any kind, express, implied or otherwise, including without limitation, any warranty of fitness for a particular purpose. In no event shall the Centers for Disease Control and Prevention (CDC) or the United States (U.S.) government be liable to you or anyone else for any direct, special, incidental, indirect or consequential damages of any kind, or any damages whatsoever, including without limitation, loss of profit, loss of use, savings or revenue, or the claims of third parties, whether or not CDC or the U.S. government has been advised of the possibility of such loss, however caused and on any theory of liability, arising out of or in connection with the possession, use or performance of this software.
