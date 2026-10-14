# Simulated help desk tickets

All names, requests, and tickets below are fictional training scenarios. No real users or production tickets were serviced. Actions were performed in the lab; this file organizes them into ticket format.

## LAB-001 — Forgotten password

**User:** Alex Morgan, IT  
**Request:** Reset a forgotten password.

**Actions:** Locate Alex in LabCorp/IT, select Reset Password, enter a private temporary password, and require a change at next logon.

**Observed result:** AD Users and Computers displayed that Alex Morgan's password had been changed.

**Validation limit:** The reset was confirmed; a client sign-in using the new password was not tested.

**Workplace consideration:** Verify the requester's identity and follow the organization's approved password-delivery process.

## LAB-002 — Account disabling and restoration

**User:** Jamie Carter, HR  
**Request:** Simulate departure, then restore the account for continued lab use.

**Actions:** Disable the account, capture confirmation, then enable it again.

**Observed result:** Confirmation messages showed both disabled and enabled status.

**Final state:** Enabled.

**Scope:** This demonstrates two account-administration actions. It is not a complete production offboarding workflow or a test of revoking existing sessions.

## LAB-003 — Department transfer

**User:** Taylor Brooks  
**Request:** Transfer from Finance to HR.

**Actions:**
1. Move the account from LabCorp/Finance to LabCorp/HR.
2. Remove GG_Finance_Users membership.
3. Add GG_HR_Users membership.
4. Retain Domain Users membership.

**Observed result:** HR showed Jamie and Taylor. Taylor's Member Of tab showed Domain Users and GG_HR_Users, with Finance membership absent.

**Lesson:** Moving an account between OUs does not automatically change group membership.

**Validation limit:** Department groups were not assigned to resources, so access to departmental data was not tested.

## LAB-004 — Limited help desk delegation

**Technician:** helpdesk.tech  
**Request:** Allow password resets and forced password changes for HR users.

**Actions:** Delegate the two related password tasks on HR to GG_HelpDesk_PasswordReset and add the technician account to that group. Test with an explicitly supplied PSCredential and a specified domain controller.

| Test | Target | Observed result |
|---|---|---|
| Outside delegated OU | alex.morgan in IT | Access denied |
| Inside delegated OU | jamie.carter in HR | Reset completed without error |
| Force password change | jamie.carter in HR | Set-ADUser completed; pwdLastSet verified as 0 |

**Outcome:** The tested password-reset boundary operated as intended.

**Troubleshooting:** An earlier MMC runas /netonly attempt reset both users and did not establish the intended boundary. Explicit PowerShell credentials were used for the accepted validation. See [troubleshooting.md](troubleshooting.md).

**Validation limit:** These tests establish the listed actions and scope; they do not constitute an exhaustive audit of every permission assigned to the technician.

