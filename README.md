SGEn issue in .NET Standard/.NET Framework build
================================================

This repository contains a sample solution that shows a bug in SGen call when in a certain situation.

The LibNetFramework is a .NET 4.6.1 targeted library that has a ProjectReference to Lib, a .NET Standard 2.0 targeted project.

In LibNetFramework, serialization assemblies generation is forced by the use of the `GenerateSerializationAssemblies` property set to `On`. Typically, this would happen when adding a WebReference to the project.

The compilation fails with the message
```
SGEN : error : An attempt was made to load an assembly with an incorrect format: C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\MSBuild\Microsoft\Microsoft.NET.Build.Extensions\net461\ref\netfx.force.conflicts.dll.
```

The issue might have been corrected by the use of the patch in https://github.com/dotnet/sdk/pull/1582, that is put ine the LibNetFrameworkPatched project.

But this breaks the build, as the netstandard.dll reference is no longer added to the compilation:

```
LibNetFrameworkPatched\Class1.cs(9,30,9,34): error CS0012: The type 'Object' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```
