
# PSWindowsUpdate Installation

## Online install PSWindowsUpdate module

Install-Module PSWindowsUpdate

### Run windows update

Get-WindowsUpdate

Install-WindowsUpdate

#### Auto reboot system after update command

Get-WindowsUpdate -AcceptAll -Install -AutoReboot

#### Download a specific update command

Get-WindowsUpdate -Install -KBArticleID 'KB5031445'

## Offline installation

Download the latest PSWindowsUpdate version from the PowerShell gallery.

https://www.powershellgallery.com/

Create a new folder named "PSWindowsUpdate" in %WINDIR%\System32\WindowsPowerShell\v1.0\Modules and extract the content of the nupkg file.

Set-ExecutionPolicy RemoteSigned 

Import-Module -Name PSWindowsUpdate.

Installed aliases and cmdlets can be displayed by typing Get-Command–module PSWindowsUpdate.

MORE: https://learn.microsoft.com/en-us/powershell/gallery/how-to/working-with-packages/manual-download?view=powershellget-3.x#using-manual-download-to-acquire-a-package

# Known issues

## unable to download from uri nuget

could be TLS security related (ref: https://rnelson0.com/2018/05/17/powershell-in-a-post-tls1-1-world/)

Try this command first:

[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12

MORE: https://learn.microsoft.com/en-us/powershell/gallery/powershellget/install-powershellget?view=powershellget-3.x#install-the-latest-stable-release

## setup proxy

Because of changes in .NET Core 3.1, PowerShell 7.0 and higher use the HttpClient.DefaultProxy Property to determine the proxy configuration.

The value of this property is determined by your platform:

For Windows: Reads proxy configuration from environment variables. If those variables aren't defined the property is derived from the user's proxy settings.
For macOS: Reads proxy configuration from environment variables. If those variables aren't defined the property is derived from the system's proxy settings.
For Linux: Reads proxy configuration from environment variables. If those variables aren't defined the property initializes a non-configured instance that bypasses all addresses.
The environment variables used for DefaultProxy initialization on Windows and Unix-based platforms are:

HTTP_PROXY: the hostname or IP address of the proxy server used on HTTP requests.
HTTPS_PROXY: the hostname or IP address of the proxy server used on HTTPS requests.
ALL_PROXY: the hostname or IP address of the proxy server used on HTTP and HTTPS requests in case HTTP_PROXY or HTTPS_PROXY aren't defined.
NO_PROXY: a comma-separated list of hostnames that should be excluded from proxying.
PowerShell 7.4 added support for the Brotli compression algorithm.

MORE: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7.4&viewFallbackFrom=powershell-3.0

## get environment variable powershell

PowerShell exposes environmental variables via the $Env: scope (you can read more about scopes here)

So to access the USERPROFILE environmental variable, you could do the following (note, I'm using Write-Host in place of echo here, see this answer for details on the difference between the two):

Write-Host $Env:USERPROFILE

PowerShell also exposes a number of automatic variables and these are made available to all scripts and commands. $HOME for example is one such automatic variable.


## PSWindowsUpdate Access Denied

Create WinRM VirtualAccount

RemoteServer:

New-PSSessionConfigurationFile -RunAsVirtualAccount -Path .\VirtualAccount.pssc
Register-PSSessionConfiguration -Name 'VirtualAccount' -Path .\VirtualAccount.pssc -Force
Get-PSSessionConfiguration -Name 'VirtualAccount'

ClientMachine:
Enter-PSSession -ComputerName <hostname> -Credential <domain\username> -ConfigurationName 'VirtualAccount'

https://www.reddit.com/r/PowerShell/comments/pcsyu4/pswindowsupdate_access_denied/


