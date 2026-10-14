# Build walkthrough

These steps describe the configuration completed in this lab. Passwords and account identifiers for the Azure subscription are intentionally excluded.

## 1. Create the Azure infrastructure

1. Create resource group `rg-active-directory-lab`.
2. Create virtual network `vnet-ad-lab` and subnet `snet-servers` using `10.0.0.0/24`.
3. Deploy VM `DC01` in Mexico Central into that subnet.
4. Select Windows Server 2025 Datacenter, Standard B2als v2, and a 127 GiB Standard SSD OS disk.
5. Use no public IP and no public inbound-port exposure.
6. Use Azure Bastion Developer to connect with the VM administrator account.
7. On NIC `dc01511` → IP configurations → `ipconfig1`, change private allocation to Static while retaining `10.0.0.4`.

The Windows network adapter remains configured through DHCP. A static address in Azure reserves the private IP; this is distinct from manually hard-coding the guest adapter address.

## 2. Install AD DS and promote DC01

In Server Manager, use Manage → Add Roles and Features:

1. Choose role-based or feature-based installation and select DC01.
2. Select Active Directory Domain Services and its required management features.
3. Install; select Promote this server to a domain controller.
4. Select Add a new forest and enter `adlab.test`.
5. Choose Windows Server 2025 forest and domain functional levels.
6. Retain DNS server and Global Catalog; do not select RODC.
7. Set a private Directory Services Restore Mode (DSRM) password.
8. Leave DNS delegation unchecked; use NetBIOS name `ADLAB`.
9. Retain database/log paths `C:\Windows\NTDS` and SYSVOL path `C:\Windows\SYSVOL`.
10. Run prerequisite checks, install, and allow the automatic restart.
11. Reconnect with the domain administrator credentials and verify `adlab.test` in Server Manager and AD Users and Computers.

This internal `.test` namespace is for the lab. The first domain controller has no existing parent DNS infrastructure for delegation.

## 3. Create the directory structure

In AD Users and Computers:

1. Create OU `LabCorp` beneath `adlab.test`, retaining accidental-deletion protection.
2. Create IT, HR, Finance, Groups, and Workstations OUs beneath LabCorp.
3. Provision these fictional accounts with private temporary passwords:

| Initial OU | Display name | Logon name |
|---|---|---|
| IT | Alex Morgan | `alex.morgan@adlab.test` |
| HR | Jamie Carter | `jamie.carter@adlab.test` |
| Finance | Taylor Brooks | `taylor.brooks@adlab.test` |

Require a password change at next logon for the employee accounts. Leave Password never expires unchecked.

## 4. Create department groups

Create Global Security groups in Groups:

| Group | Initial member |
|---|---|
| `GG_IT_Users` | Alex Morgan |
| `GG_HR_Users` | Jamie Carter |
| `GG_Finance_Users` | Taylor Brooks |

Use group Properties → Members → Add → Check Names to select and save each member. HR and Finance initial membership completion was reported during the lab; Alex's membership screenshot was reviewed directly.

## 5. Perform account-management scenarios

Perform the scenarios in [help-desk-tickets.md](help-desk-tickets.md). Final state: Jamie is enabled; Taylor is in HR and belongs to GG_HR_Users rather than GG_Finance_Users.

## 6. Delegate HR password resets

1. Create Global Security group `GG_HelpDesk_PasswordReset` in Groups.
2. Right-click the HR OU → Delegate Control.
3. Select that group.
4. Select only Reset user passwords and force password change at next logon; finish the wizard.
5. Create Helpdesk Technician in IT with logon name `helpdesk.tech`.
6. For this test account, use a private password and leave forced change at next logon unchecked to allow credential-based testing. Password never expires remains unchecked.
7. Add helpdesk.tech to GG_HelpDesk_PasswordReset.
8. Test using explicit PowerShell credentials as described in [validation.md](validation.md).

The scope is HR, not the domain root or all of LabCorp. Do not add the technician to Domain Admins.

## 7. Validate AD and DNS

Run the targeted dcdiag checks and DNS lookups in [validation.md](validation.md). All listed checks produced the expected results.

## 8. Configure the workstation GPO

1. Open Server Manager → Tools → Group Policy Management.
2. Expand Forest: adlab.test → Domains → adlab.test → LabCorp.
3. Right-click Workstations → Create a GPO in this domain, and Link it here.
4. Name it `LabCorp - Workstation Screen Lock`.
5. Edit it and navigate to Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Security Options.
6. Define Interactive logon: Machine inactivity limit as 900 seconds.
7. Apply and verify the saved setting.

The Workstations link is enabled and GPO status is Enabled. Enforced was No. No client was available to test actual enforcement.

