# Evidence and publishing guide

## Screenshots to add

The original photos remain separate from this package. Use the mappings below to locate them, crop to the relevant application/output, and save public copies with descriptive names. Browser address bars may include session URLs; exclude them. Also exclude account email, subscription/tenant IDs, credentials, and unrelated personal details. Private lab addresses and the fictional internal domain may remain visible.

| Suggested public filename | Original photo | Evidence |
|---|---|---|
| 01-static-private-ip.jpg | IMG_41CF041E-6CBA-4D9A-AF51-7B988024A2F7.jpeg | Azure private IP 10.0.0.4 Static |
| 02-domain-created.jpg | IMG_B14E79BA-6C43-4DE6-89A3-9C12D2298BDF.jpeg | DC01 domain adlab.test |
| 03-ou-structure.jpg | IMG_B817C49F-DA44-4A2F-B00E-7B3AFFE16D5E.jpeg | LabCorp child OUs |
| 04-department-groups.jpg | IMG_80C40CCB-8E00-4756-A299-90B4B432FAC4.jpeg | Department security groups |
| 05-it-membership.jpg | IMG_D87444A2-48F4-4F7E-966C-85D165007735.jpeg | Alex in GG_IT_Users |
| 06-password-reset.jpg | IMG_CA39C7A8-58E8-44CE-877D-AC0957EA073B.jpeg | Alex reset confirmation |
| 07-account-disabled.jpg | IMG_62CED4DF-CE29-462F-87FD-6F883E01E4ED.jpeg | Jamie disabled |
| 08-account-enabled.jpg | IMG_6DCB6E07-0F60-43E2-BA52-E4810554D987.jpeg | Jamie enabled |
| 09-transfer-ou.jpg | IMG_36B22D25-794A-4B7C-9A10-62F034414338.jpeg | Taylor in HR |
| 10-transfer-membership.jpg | IMG_03687836-AED5-482E-8263-D4E931FB937D.jpeg | Taylor's HR membership |
| 11-delegation-boundary.jpg | IMG_49738866-5065-47A5-90E1-2F22F950FAC9.jpeg | Explicit-credential IT denial and HR success |
| 12-required-password-change.jpg | IMG_005E5FB2-CEF0-4D22-96EB-6CFBD5EBFA0A.jpeg | Jamie pwdLastSet 0 |
| 13-dc-health.jpg | IMG_9A4C8A48-8A70-47A6-9389-FD17B6FA4608.jpeg | Four dcdiag checks passed |
| 14-dns-a-record.jpg | IMG_423F4F80-EA47-42BF-8AC9-56D7AD0D7212.jpeg | DC01 A record |
| 15-dns-srv-record.jpg | IMG_B2CB92AA-8532-4E49-90A9-3B390443EEBA.jpeg | Corrected LDAP SRV response |
| 16-gpo-link.jpg | IMG_767A634F-8B78-4928-AE0F-11F4F74A90C9.jpeg | Workstations GPO link enabled |
| 17-gpo-setting.jpg | IMG_4E0A00E7-1839-4FA3-86CD-4FDEB8A7B554.jpeg | Inactivity limit 900 seconds |

For photos with old errors above the successful result, crop to the accepted output while retaining its command and prompt when possible. Do not alter the substantive result. A few strong screenshots are sufficient; publishing every wizard page is unnecessary.

## Optional screenshots still to capture

- Resource group resource inventory.
- Virtual network/subnet overview.
- Helpdesk Technician membership in the delegation group.
- Full GPO name and saved setting in the Settings report.

These are optional publication improvements, not additional lab exercises. Existing evidence supports the completed steps; do not invent missing screenshots.

## GitHub publication

Suggested repository name: `active-directory-azure-lab`.

1. Extract the ZIP.
2. Create a repository using that name and a description such as: “Azure Windows Server AD DS lab: user administration, help desk scenarios, limited delegation, DNS validation, and GPO configuration.”
3. Upload README.md, docs/, and reviewed screenshot copies in screenshots/.
4. In README.md, replace the Evidence paragraph with links to the screenshots you actually added. Example after the matching file exists:

```markdown
![Help desk delegation boundary](screenshots/11-delegation-boundary.jpg)
```

5. Preview the README and confirm its diagrams and relative links work.
6. Keep personal lab work under Projects on the résumé. Add the real repository URL to LinkedIn when available.

This package prepares content; it does not create a repository, publish a post, or update a résumé/account.

## Claim review

Use “configured a workstation screen-lock GPO.” Do not use “enforced locking on workstations” until a joined client is tested. Use “simulated help desk scenarios,” not real tickets or production support. No file-share permissions, account unlock exercise, redundant DCs, or Entra ID deployment were completed.

