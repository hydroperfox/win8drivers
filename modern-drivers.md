# Installing modern drivers in a previous Windows version

## Export drivers from existing Windows installation into a folder

In the command prompt, run the following, substituting the `<full folder path>` sequence by the directory path:

```batch
dism /online /export-driver /destination:<full folder path>
```

You may then pass these drivers to a flash drive.

## Disable driver signature enforcement

To disable driver signature enforcement, access the Recovery > Advanced Settings > Advanced Startup Options in the Boot menu.

## Install all drivers in a folder through a PowerShell .ps1 script

1. Save a file installdrivers.ps1 with the contents, substituting `<full folder path>` by the directory path containing INF driver files:

```
Get-ChildItem "<full folder path>" -Recurse -Filter "*.inf" | 
ForEach-Object { PnPutil.exe -i -a $_.FullName }
```

2. Open PowerShell as administrator and run:

```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
```

3. `Browse to the directory containing the .ps1 script in PowerShell with the `cd` command and run it through

```
.\installdrivers.ps1
```

Accept driver installation in every popup.
