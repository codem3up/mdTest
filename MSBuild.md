# CWMasterTeacher MsBuild Guide

## Overview
This document will give a rough overview of MSBuild, as well as how it applies to the CWMasterTeacher project, and a brief summary of the basics of MSBuild a long with some real examples.  MsBuild is a build tool set for managing software.  It can build a project against many supported .NET framework versions.  It is built into and included with Visual studio, however it can be invoked through the command line as well.  MsBuild has an XML like syntax, and looks much like other build based tools like NANT.  MsBuild files come with the extension .msbuild.  Our current 


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

### Item Groups


