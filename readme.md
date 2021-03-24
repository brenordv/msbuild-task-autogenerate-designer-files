# MsBuild Task: Auto generate designer files
This is a script to be used on PreBuild events. It will create a designer file for each RESX file that it encounters.
I've used a dependency-free approach. So it should work on any platform that can execute PowerShell scripts.

If necessary, the templates can be altered to fit another format. For now, it works fine.


# How to use
## Add the following in your .csproj
Consider that your script is in: ```[solution folder]\BuidTasks```
```xml
    <Target Name="PreBuild" BeforeTargets="PreBuildEvent">        
        <Exec Command="powershell.exe -ExecutionPolicy Bypass -NoProfile -NonInteractive -File ..\BuildTasks\Create-DesignerFiles.ps1  -path $(ProjectDir) -namespace Project.Namespace -backup" />
    </Target>
```
