# Get Windows computer information

## GUI: winver

winver

## CMD: systeminfo

systeminfo = Get-ComputerInfo

Get-ComputerInfo | select OsName, OsVersion, WindowsVersion, OsBuildNumber, OsArchitecture

### Command output can be filtered:

systeminfo | findstr /B /C:"OS Name" /B /C:"OS Version"

### Or use the WMI command:

wmic os get Caption, Version, BuildNumber, OSArchitecture

## CMD: env

### Get the Windows version with the environment variable:

[System.Environment]::OSVersion.Version

### From the WMI class:

Get-WmiObject -Class Win32_OperatingSystem | fl -Property Caption, Version, BuildNumber
Get-CimInstance Win32_OperatingSystem | fl -Property Caption, Version, BuildNumber, OSArchitecture

## CMD: registry

Reg Query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion" /v ProductName
Reg Query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion" /v DisplayVersion
Reg Query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion" /v CurrentBuild

Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion"| select ProductName, DisplayVersion, CurrentBuild

Use PowerShell Remoting to check the Windows version on remote hosts:

Invoke-Command -ScriptBlock {Get-ItemProperty 'HKLM:SOFTWARE\Microsoft\Windows NT\CurrentVersion' | Select-Object ProductName, ReleaseID, CurrentBuild} -ComputerName dskt-pc01

Or use WMI/CIM:

Get-ciminstance Win32_OperatingSystem -ComputerName dskt-pc01 | Select PSComputerName, Caption, OSArchitecture, Version, BuildNumber | FL
