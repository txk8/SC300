
Setup

Use a test or lab tenant, as you could accidentally lock yourself out. You'll also need either an Entra P2 license or Microsoft 365 E5 trial license.

1) Turn off security defaults. 

Entra admin center → Entra ID → Overview → Properties → Manage security defaults → Disabled. Conditional Access policies can’t be created while they’re on.

<img width="419" height="211" alt="image" src="https://github.com/user-attachments/assets/aca089f7-4706-4d8e-890d-5a89dc847713" />


Create test identities:
breakglass: a cloud-only Global Administrator. Exclude it from every policy you build.
testuser1 and testuser2, both in a group called CA-Test-Users.
testadmin: assigned a role such as Helpdesk Administrator.
Find your public IP address, since you’ll need it in step 1.


