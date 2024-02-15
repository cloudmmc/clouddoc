






#

## Refresh membership in AD security groups without reboot or logoff

### Purge computer account ticket

klist.exe sessions | findstr /i %COMPUTERNAME%

klist.exe -li 0x3e7

klist.exe -li 0x3e7 purge

### Verify computer account group membership

gpupdate /force

gpresult /r

gpresult /r /scope computer


