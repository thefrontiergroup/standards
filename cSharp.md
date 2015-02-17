# C Sharp

## Code Style

C# Projects have style enforced by [StyleCop] (https://stylecop.codeplex.com/). These rules are available in [tooling] (/tooling/cSharp) 

Notable rules enforced are:

* Naming conventions. 
 * Classes and Structs are `UpperCamelCased`
 * Interfaces are `UpperCamelCased` and prefixed with `I`  (`ISomeInterface`) 
 * Properties are `UpperCamelCased`
 * Methods are `UpperCamelCased`
 * Fields are `_underscoreLowerCamelCased`
 
 * Members be ordered Static to Instance then Public to Private

* A target must be specifed for all non local access calls. This means `this` must be used for instance methods / properties / fields, and `SomeClassName` must be specified for static methods.

```c#
this.DoSomething();
MyClass.DoSomethingStatic();
```

## Static Analysis

Currently [FxCop] (https://msdn.microsoft.com/en-us/library/bb429476.aspx) is not used to detect static issues, however a draft settings file can be found in [tooling/cSharp/FxCop.ruleset] (/tooling/cSharp)

## Code Contracts

Code contracts are required for all projects. The enforcment level should be at a minimum of three: 
![Code Contract Level] (/assets/cSharp/CodeContractLevel.jpg)
