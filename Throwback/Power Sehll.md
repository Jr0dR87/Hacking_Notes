### Powershell

Powershell is the Windows Scripting Language and shell environment that is built using the .NET framework.

cmdlets (which are written in .NET) allow Powershell to execute .NET functions directly from the shell.

Common Verbs in Power shell are Get, Start, Stop, Read, Write, New, and Out.

Get-Help displays information about a cmdlet.
```powershell
Get-Help Command-Name
```

Get-Command gets all the cmdlets installed on the current device.

```powershell
Verb-Noun | Get-Member    
```

#### Filtering Objects

```powershell
Verb-Noun | Where-Object -Property PropertyName -operator Value
```

#### Sort Objects

```powershell
Verb-Noun | Sort-Object
```

#### Get-ADDomain

Get-ADDomain is a commandlet that pulls a large majority of the information about the Domain
```powershell
Get-ADDomain | Select-Object NetBIOSName, DNSRoot, InfrastructureMaster
```
