# MsBuild Task: Auto generate designer files
This is a script to be used on PreBuild events. It will create a designer file for each RESX file that it encounters.
I've used a dependency-free approach. So it should work on any platform that can execute PowerShell scripts.

If necessary, the templates can be altered to fit another format. For now, it works fine.

# Be advised
if there's no Designer.cs files, the build process will fail, but the files will be generated. you'll just have to run it again.
Depending on your build order, you can bypass this. However, without a global (solution-wide) pre-build event, i don't see a workaround for that.

# How to use
## Add the following in your .csproj
Consider that your script is in: ```[solution folder]\BuidTasks```
```xml
    <Target Name="PreBuild" BeforeTargets="PreBuildEvent">        
        <Exec Command="powershell.exe -ExecutionPolicy Bypass -NoProfile -NonInteractive -File ..\BuildTasks\Create-DesignerFiles.ps1  -path $(ProjectDir) -namespace Project.Namespace -backup" />
    </Target>
```
Note: If you do this, you will change source (only .Designer.cs) files during release pipeline and this might affect your release process.

Running only on Debug:
```xml
    <Target Name="PreBuild" BeforeTargets="PreBuildEvent" Condition="'$(Configuration)'=='Debug'">
        <Exec Command="powershell.exe -ExecutionPolicy Bypass -NoProfile -NonInteractive -File ..\BuildTasks\Create-DesignerFiles.ps1  -path $(ProjectDir) -namespace Project.Namespace -backup" />
    </Target>
```

If your path contains spaces, you should add double quotes to the paths:
```xml
    <Target Name="PreBuild" BeforeTargets="PreBuildEvent" Condition="'$(Configuration)'=='Debug'">
        <Exec Command="powershell.exe -ExecutionPolicy Bypass -NoProfile -NonInteractive -File &quot;..\BuildTasks\Create-DesignerFiles.ps1&quot;  -path &quot; $(ProjectDir) &quot; -namespace Project.Namespace -backup" />
    </Target>
```
Note: You cannot add &quot; to the end of $(ProjectDir) variable without a space, because it ends with a \ and if you add the ", it will be escaped and the script will fail.

## Run as a common PowerShell script
```shell
.\Create-DesignerFiles.ps1  -path c:\path\to\project\containing\resxfiles -namespace Project.Namespace 
```
