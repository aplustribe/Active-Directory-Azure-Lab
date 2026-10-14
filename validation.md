# Validation results

Run dates: October 13, 2026. Commands were executed manually inside DC01. Results below are based on the lab checkpoint screenshots, not invented output.

## Explicit credential setup

Do not put passwords in scripts or command history. In the same Windows PowerShell session:

```powershell
$helpdeskPassword = Read-Host 'Enter helpdesk.tech password' -AsSecureString
$helpdeskCred = [System.Management.Automation.PSCredential]::new('ADLAB\helpdesk.tech', $helpdeskPassword)
$helpdeskCred.UserName
```

The username displayed ADLAB\helpdesk.tech. That display identifies the credential object; the subsequent AD requests validate the supplied credentials.

## Negative test: IT reset

```powershell
$newPassword = Read-Host 'New temporary password for Alex' -AsSecureString
Set-ADAccountPassword -Identity 'alex.morgan' -Reset -NewPassword $newPassword -Credential $helpdeskCred -Server 'DC01.adlab.test' -ErrorAction Stop
```

**Observed:** Access is denied / PermissionDenied. The attempted reset did not succeed.

## Positive test: HR reset and required change

```powershell
$hrPassword = Read-Host 'New temporary password for Jamie' -AsSecureString
Set-ADAccountPassword -Identity 'jamie.carter' -Reset -NewPassword $hrPassword -Credential $helpdeskCred -Server 'DC01.adlab.test' -ErrorAction Stop
Set-ADUser -Identity 'jamie.carter' -ChangePasswordAtLogon $true -Credential $helpdeskCred -Server 'DC01.adlab.test' -ErrorAction Stop
```

**Observed:** Both final corrected commands returned to the prompt without errors.

A readback in the administrator session:

```powershell
Get-ADUser 'jamie.carter' -Properties pwdLastSet | Select-Object SamAccountName,pwdLastSet
```

**Observed:** jamie.carter, pwdLastSet 0, confirming the required change at next logon. Actual client logon/password change remains untested.

## Targeted domain-controller diagnostics

```powershell
dcdiag /test:Advertising /test:Services /test:SysVolCheck /test:NetLogons
```

| Test | Result |
|---|---|
| Advertising | DC01 passed |
| Services | DC01 passed |
| SysVolCheck | DC01 passed |
| NetLogons | DC01 passed |

These selected checks passed. This is not a claim that every dcdiag test, disaster recovery, or replication between multiple DCs was tested.

## DNS queries

```powershell
Resolve-DnsName DC01.adlab.test -Type A -Server 10.0.0.4
Resolve-DnsName _ldap._tcp.dc._msdcs.adlab.test -Type SRV -Server 10.0.0.4
```

| Record | Observed result |
|---|---|
| DC01 A | 10.0.0.4 |
| LDAP DC SRV | Target dc01.adlab.test; port 389; priority 0; weight 100 |
| Additional A in SRV response | dc01.adlab.test → 10.0.0.4 |

This verifies responses from DC01's DNS server. DNS configuration and connectivity from a separate client have not been tested.

## GPO configuration verification

| Property | Verified value |
|---|---|
| Name entered during creation | LabCorp - Workstation Screen Lock |
| Link | LabCorp/Workstations |
| Link enabled | Yes |
| GPO status | Enabled |
| Enforced | No |
| Computer security setting | Interactive logon: Machine inactivity limit |
| Value | 900 seconds |

The saved editor setting was visible. No computer is in Workstations, so application and actual lock behavior remain untested. DC01 remains in Domain Controllers; it was not moved into Workstations to force this test.

## Future client validation — not yet performed

After provisioning a suitable lab client, configure its DNS to use DC01, join adlab.test, and place its computer account in Workstations. Then refresh policy, inspect gpresult, and test inactivity locking. Record the actual results before claiming enforcement. This expansion was not required to complete the current configuration exercise.

