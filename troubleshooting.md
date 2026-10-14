# Troubleshooting notes

## Azure VM agent was not ready

**Observed:** Azure displayed a VM-agent-not-ready warning.  
**What worked:** Bastion access was successful, and in-guest lab work continued.  
**Status:** Agent readiness was not subsequently verified. Do not describe the warning as repaired or closed.

## Bastion target unreachable after restarts

**Observed:** Bastion displayed an unreachable-target message after guest restarts, including a generic NSG instruction mentioning ports 22/3389 and 168.63.129.16.  
**Action:** Allow the restart to finish and reconnect.  
**Result:** Later connections succeeded without an NSG change.

The message alone did not establish an NSG fault. No public RDP exposure or Bastion upgrade was needed.

## Pending restart before role installation

**Observed:** Add Roles and Features showed a pending restart.  
**Action:** Restart Windows, reconnect, and reopen the wizard.  
**Result:** The pending-restart banner cleared.

## Certificate Services selected accidentally

**Observed:** Active Directory Certificate Services was selected during an early wizard attempt.  
**Action:** Correct the role to Active Directory Domain Services and remove leftover Certificate Services management-tool selections before installation.  
**Result:** The accepted installation contained AD DS, Group Policy Management, and AD DS administration tools.

## Promotion warnings

**DNS delegation:** No authoritative parent zone was available for the new internal lab namespace. DNS delegation remained unchecked; the prerequisite checks passed.

**Static-IP warning:** Windows retained DHCP while Azure NIC allocation was static at 10.0.0.4. Microsoft describes this Azure configuration and the guest static-IP warning in its [AD DS on Azure guidance](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm).

## Initial delegation test was inconclusive

An MMC window launched with runas /netonly successfully reset both an HR and an IT account. That did not validate the HR-only boundary. /netonly retains the current local identity and supplies alternate credentials for network access; the exact reason the local MMC operation used broader rights was not conclusively diagnosed.

The accepted test used Set-ADAccountPassword with an explicit -Credential and -Server. With the same technician credentials, Alex's IT reset was denied and Jamie's HR reset succeeded. Use that paired test as portfolio evidence.

## Command-entry issues

| Issue | Correction |
|---|---|
| mm.exe could not be found | Use mmc.exe |
| Get-Credential returned no saved username | Read a SecureString and explicitly construct PSCredential |
| Set-ADUser parameter-binding error | Include -Credential before $helpdeskCred |
| Missing Server argument | Include DC01.adlab.test after -Server |
| SRV query returned name-not-found | Correct _ldap: lowercase L, not uppercase I |

A command syntax error is not evidence of denied authorization. Correct the syntax before interpreting the result.

## Evidence standard

Keep confirmed results distinct from assumptions. The PowerShell permission-boundary test supersedes the ambiguous MMC attempt. The GPO editor confirms saved configuration, not client enforcement.

