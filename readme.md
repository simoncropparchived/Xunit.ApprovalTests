<!--
GENERATED FILE - DO NOT EDIT
This file was generated by [MarkdownSnippets](https://github.com/SimonCropp/MarkdownSnippets).
Source File: /readme.source.md
To change this file edit the source file and then run MarkdownSnippets.
-->

# <img src="/src/icon.png" height="30px"> ApprovalTests.Net.Xunit

[![Build status](https://ci.appveyor.com/api/projects/status/5x0gasnuhtblvcv2/branch/master?svg=true)](https://ci.appveyor.com/project/SimonCropp/ApprovalTests.Net.Xunit)
[![NuGet Status](https://img.shields.io/nuget/v/ApprovalTests.Net.Xunit.svg?cacheSeconds=86400)](https://www.nuget.org/packages/ApprovalTests.Net.Xunit/)


The default behavior of ApprovalTests uses the [StackTrace](https://docs.microsoft.com/en-us/dotnet/api/system.diagnostics.stacktrace) to derive the current test and hence compute the name of the approval file. This has several drawbacks/issues:

 * Fragility: Deriving the test name from a stack trace is dependent on several things to be configured correctly. Optimization must be disabled to avoid in-lining and debug symbols enabled and parsable.
 * Performance impact: Computing a stack trace is a relatively expensive operation. Disabling optimization also impacts performance

ApprovalTests.Net.Xunit avoids these problems by using the current xUnit context to derive the approval file name.


<!-- toc -->
## Contents

  * [NuGet package](#nuget-package)
  * [Usage](#usage)
  * [xUnit Theory](#xunit-theory)
  * [AsEnvironmentSpecificTest](#asenvironmentspecifictest)
  * [UseApprovalSubdirectory](#useapprovalsubdirectory)
  * [ForScenario](#forscenario)
  * [Base Class](#base-class)<!-- endtoc -->


## NuGet package

https://nuget.org/packages/ApprovalTests.Net.Xunit/


## Usage

Usage is done via inheriting from a base class `XunitApprovalBase`

<!-- snippet: XunitApprovalBaseUsage -->
<a id='snippet-xunitapprovalbaseusage'/></a>
```cs
public class Sample :
    XunitApprovalBase
{
    [Fact]
    public void Simple()
    {
        Approvals.Verify("SimpleResult");
    }

    public Sample(ITestOutputHelper testOutput) :
        base(testOutput)
    {
    }
```
<sup><a href='/src/Tests/Snippets/Sample.cs#L6-L20' title='File snippet `xunitapprovalbaseusage` was extracted from'>snippet source</a> | <a href='#snippet-xunitapprovalbaseusage' title='Navigate to start of snippet `xunitapprovalbaseusage`'>anchor</a></sup>
<!-- endsnippet -->


## xUnit Theory

[xUnit Theories](https://xunit.net/docs/getting-started/netfx/visual-studio#write-first-theory) are supported.

<!-- snippet: Theory -->
<a id='snippet-theory'/></a>
```cs
[Theory]
[InlineData("Foo")]
[InlineData(9)]
[InlineData(true)]
public void Theory(object value)
{
    Approvals.Verify(value);
}
```
<sup><a href='/src/Tests/Snippets/Sample.cs#L53-L62' title='File snippet `theory` was extracted from'>snippet source</a> | <a href='#snippet-theory' title='Navigate to start of snippet `theory`'>anchor</a></sup>
<!-- endsnippet -->

Will result in the following `.approved.` files:

 * `Sample.Theory_value=Foo.approved.txt`
 * `Sample.Theory_value=9.approved.txt`
 * `Sample.Theory_value=True.approved.txt`


## AsEnvironmentSpecificTest

ApprovalTests `NamerFactory.AsEnvironmentSpecificTest` is supported.

<!-- snippet: AsEnvironmentSpecificTest -->
<a id='snippet-asenvironmentspecifictest'/></a>
```cs
[Fact]
public void AsEnvironmentSpecificTest()
{
    using (NamerFactory.AsEnvironmentSpecificTest("Foo"))
    {
        Approvals.Verify("Value");
    }
}
```
<sup><a href='/src/Tests/Snippets/Sample.cs#L31-L40' title='File snippet `asenvironmentspecifictest` was extracted from'>snippet source</a> | <a href='#snippet-asenvironmentspecifictest' title='Navigate to start of snippet `asenvironmentspecifictest`'>anchor</a></sup>
<!-- endsnippet -->

Will result in the following `.approved.` file:

 * `Sample.AsEnvironmentSpecificTest_Foo.approved.txt`


## UseApprovalSubdirectory

ApprovalTests `[UseApprovalSubdirectory]` is supported.

<!-- snippet: UseApprovalSubdirectory -->
<a id='snippet-useapprovalsubdirectory'/></a>
```cs
[Fact]
[UseApprovalSubdirectory("SubDir")]
public void InSubDir()
{
    Approvals.Verify("SimpleResult");
}
```
<sup><a href='/src/Tests/Snippets/Sample.cs#L22-L29' title='File snippet `useapprovalsubdirectory` was extracted from'>snippet source</a> | <a href='#snippet-useapprovalsubdirectory' title='Navigate to start of snippet `useapprovalsubdirectory`'>anchor</a></sup>
<!-- endsnippet -->

Will result in the following `.approved.` file:

 * `SubDir\Sample.InSubDir.approved.txt`


## ForScenario

ApprovalTests `ApprovalResults.ForScenario` is supported.

<!-- snippet: ForScenario -->
<a id='snippet-forscenario'/></a>
```cs
[Fact]
public void ForScenarioTest()
{
    using (ApprovalResults.ForScenario("Name"))
    {
        Approvals.Verify("Value");
    }
}
```
<sup><a href='/src/Tests/Snippets/Sample.cs#L42-L51' title='File snippet `forscenario` was extracted from'>snippet source</a> | <a href='#snippet-forscenario' title='Navigate to start of snippet `forscenario`'>anchor</a></sup>
<!-- endsnippet -->

Will result in the following `.approved.` file:

 * `Sample.ForScenarioTest_ForScenario.Name.approved.txt`


## Base Class

When creating a custom base class for other tests, it is necessary to pass through the source file path to `XunitApprovalBase` via the constructor.

<!-- snippet: XunitApprovalCustomBase -->
<a id='snippet-xunitapprovalcustombase'/></a>
```cs
public class CustomBase :
    XunitApprovalBase
{
    public CustomBase(
        ITestOutputHelper testOutput,
        [CallerFilePath] string sourceFile = "") :
        base(testOutput, sourceFile)
    {
    }
}
```
<sup><a href='/src/Tests/Snippets/CustomBase.cs#L4-L15' title='File snippet `xunitapprovalcustombase` was extracted from'>snippet source</a> | <a href='#snippet-xunitapprovalcustombase' title='Navigate to start of snippet `xunitapprovalcustombase`'>anchor</a></sup>
<!-- endsnippet -->


## Release Notes

See [closed milestones](../../milestones?state=closed).


## Icon

[Wolverine](https://thenounproject.com/term/wolverine/18415/) designed by [Mike Rowe](https://thenounproject.com/itsmikerowe/) from [The Noun Project](https://thenounproject.com/).
