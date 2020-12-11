---
layout: post
title: MSBuild: Using regex in condition
date: 2020-12-11 18:40
tags:
- msbuild
- regex
---

## Condition with regex

Add properties for all `XToolkit` projects that do not contain "Tests" in the name:

```xml
<PropertyGroup
    Condition="$([System.Text.RegularExpressions.Regex]::IsMatch($(ProjectName), '^XToolkit.(.*)(?&lt;!Tests)$'))">
    <!-- Your properties to add -->
</PropertyGroup>
```
Result:
```
XToolkit.ProjectA - add
XToolkit.ProjectA.SubProjectA - ok
XToolkit.ProjectA.Tests - ignore
XToolkit.ProjectB - add
XToolkit.Tests - ignore
```

## Condition debug

- Add new target to project:

```xml
<Target Name="MyTarget">
    <Message 
      Condition="...your condition..."
      Text="The value of SomeVariable is: MyTarget" Importance="High" />
</Target>
```

- Run target:

```
msbuild ExampleProject.csproj /t:MyTarget
```
