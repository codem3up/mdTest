# CWMasterTeacher MsBuild Guide

## Overview
This document will give a rough overview of MSBuild, as well as how it applies to the CWMasterTeacher project, and a brief summary of the basics of MSBuild a long with some real examples.  MsBuild is a build tool set for managing software.  It can build a project against many supported .NET framework versions.  It is built into and included with Visual studio, however it can be invoked through the command line as well.  MsBuild has an XML like syntax, and looks much like other build based tools like NANT.  It can be seen as a basic scripting language that can execute commands of files that come with the extension .msbuild.


## The Basics

### Project
The very outermost tags in an MSBuild script are the Project Tags.  The project tag is used to highlight where the MSBuild Script begins and ends.  Everything should be placed inside these tags. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project DefaultTargets="BuildSolution" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  // All other tags go inside the Project Tag
</Project>
```

### Targets 
Targets are a key concept when it comes to MSBuild files.  Targets are basically small encapsulated snippets of commands or instructions that get executed when you invoke them.  When you invoke some target, all commands that are within that Targets markup will be executed.  Here's a very simple example of an MSBuild Target

```xml
	<Target Name="RestorePackages">
		<Exec Command="nuget.exe restore CWMasterTeacher3.sln" />
	</Target>
```
This target is pretty simple to understand.  It has a name of RestorePackages, and an execute instruction.  An execute instruction will execute a command like how you would execute a command in the terminal.  In this case the command calls nuget in order to restore all of our packages for the CWMasterTeacher solution.  

Here's another example of a target:

```xml
	<Target Name="BuildSolution" DependsOnTargets="RestorePackages">
		<MSBuild Projects="CWMasterTeacher3.sln" Properties="Configuration=$(Configuration);Platform=$(Platform)"/>
	</Target>
```
As you can see, this target has an additional attribute called "RestorePackages".  This attribute is useful because it means that this target will ensure that the RestorePackages target (The one we mentioned earlier) is invoked before running the BuildSolution target.  This allows you to build hierarchies of targets which have dependencies on other targets.  Inside this target we have a command that builds our CWMasterTeacher project (It basically invokes a different MSBuild script that is included in our project)  

### Property Groups
A property group is simply a group of properties.  Properties are user defined property names with values (Just like variables) you can use them for various reasons, they can be used to hold paths, or just values that you need throughout your MSBuild file.  An example of a simple property group contained in the CwMasterTeacher build file is below:

```xml
	<PropertyGroup>
		<Configuration>Debug</Configuration>
		<Platform>Any CPU</Platform>
	</PropertyGroup>
```

### Item Groups
The final type of tags we're going to cover in this guide is Item Groups.  Item groups contain a set of user defined item elements.  The MSDN documentation on Items provides a good explanation "Item elements define inputs into the build system, and are grouped into item collections based on their user-defined collection names. These item collections can be used as parameters for tasks, which use the individual items in the collections to perform the steps of the build process. For more information, see [Items](https://msdn.microsoft.com/en-us/library/ms171453.aspx)." 

Below is an example of an ItemGroup in our MsBuild file
```xml
	<ItemGroup>
		<TestingSupportBuildFile Include="$(MSBuildStartupDirectory)\TestingSupport.msbuild" />
	</ItemGroup>
```
This ItemGroup contains an item that Includes our TestingSupport msbuild file (So that we can call Targets on the included MsBuild task to perform functions that the other MsBuild file defines. 

That covers a lot of how our current MsBuild script works and what the commands and syntax looks like.  Pretty neat huh?  



