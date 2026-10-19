
Setup

Use a test or lab tenant, as you could accidentally lock yourself out. You'll also need either an Entra P2 license or Microsoft 365 E5 trial license.

1) Turn off security defaults. 

Entra admin center → Entra ID → Overview → Properties → Manage security defaults → Disabled. Conditional Access policies can’t be created while they’re on.

<img width="419" height="211" alt="image" src="https://github.com/user-attachments/assets/aca089f7-4706-4d8e-890d-5a89dc847713" />


2) Create test identities:
breakglass: a cloud-only Global Administrator. Exclude it from every policy you build.

<img width="189" height="245" alt="image" src="https://github.com/user-attachments/assets/352533c5-6fce-4601-a7fd-5ee46b953dd3" />

Sam and Rob, both in a group called CA-Test-Users.

<img width="663" height="240" alt="image" src="https://github.com/user-attachments/assets/b02efe10-6831-451e-90c5-3f3b3ed370e5" />

testadmin: assigned a role such as Helpdesk Administrator.

Find out your public IP address, you can google what is my IP or use ifconfig. Go onto Conditional Access -> Named Locations -> Add IP ranges location -> add your IP and range e.g. /32

<img width="388" height="292" alt="image" src="https://github.com/user-attachments/assets/2101907c-3602-4643-91d5-02430ced8035" />


Creating a report-only Policy

1) Named Location. Open Conditional Access -> Create a new policy -> configure each section with the following:
   
| Section | Setting | Value |
|---|---|---|
| Name | Policy name | `CA-MFA-Outside-Trusted-Network` |
| Users | Include | Select users and groups: `CA-Test-Users` |
| Users | Exclude | `breakglass` |
| Target resources | Include | All resources (formerly "All cloud apps") |
| Conditions | Locations / Network: Configure | Yes |
| Conditions | Locations / Network: Include | Any network or location |
| Conditions | Locations / Network: Exclude | All trusted networks and locations |
| Grant | Control | Grant access, then Require multifactor authentication |
| Enable policy | Mode | Report-only |

 <img width="304" height="347" alt="image" src="https://github.com/user-attachments/assets/2c1fe649-c97a-4ffa-8c08-9e066de9794c" />

### Left as Default Values 

User risk, Sign-in risk, Insider risk, Device platforms, Client apps, Filter for devices, Authentication flows, Session controls.

Policy 2 - Block Legacy Authentication

You should consider blocking legacy authentication as many older protocols send a username and password. In the scenario where someone has a stolen or sprayed set of credentials, they can skip MFA policies completely. 

## Conditional Access Policy: Block Legacy Authentication

| Section | Setting | Value |
|---|---|---|
| Name | Policy name | `CA-Block-Legacy-Auth` |
| Users | Include | All users |
| Users | Exclude | `breakglass` |
| Target resources | Include | All resources |
| Conditions | Client apps: Configure | Yes |
| Conditions | Client apps: tick only | Exchange ActiveSync clients, Other clients |
| Conditions | Client apps: leave unticked | Browser, Mobile apps and desktop clients |
| Grant | Control | Block access |
| Enable policy | Mode | Report-only |

<img width="306" height="198" alt="image" src="https://github.com/user-attachments/assets/b84c536b-5841-4a75-bb67-8bfbae99312a" />

Policy 3 - Phishing Resistant MFA for Admins

## Conditional Access Policy: Phishing-Resistant MFA for Admins

| Section | Setting | Value |
|---|---|---|
| Name | Policy name | `CA-Admins-Phishing-Resistant-MFA` |
| Users | Include | Select users and groups -> Directory roles: Global Administrator, Helpdesk Administrator (include whichever role `testadmin` holds) |
| Users | Exclude | `breakglass` |
| Target resources | Include | All resources |
| Conditions | All | Not configured (default) |
| Grant | Control | Grant access, then Require authentication strength -> Phishing-resistant MFA |
| Enable policy | Mode | Report-only |

<img width="306" height="463" alt="image" src="https://github.com/user-attachments/assets/b44e217e-ba94-44ca-968c-bbda8523c9d1" />

Policy 4 - MFA for Guest Users

## Conditional Access Policy: MFA for Guest Users

| Section | Setting | Value |
|---|---|---|
| Name | Policy name | `CA-Guests-MFA` |
| Users | Include | Select users and groups -> Guest or external users (tick all the sub-types, or at least B2B collaboration guest users) |
| Users | Exclude | `breakglass` (as a habit, even though it isn't a guest) |
| Target resources | Include | All resources |
| Conditions | All | Not configured (default) |
| Grant | Control | Grant access, then Require multifactor authentication |
| Enable policy | Mode | Report-only |

### Cross-tenant access settings (inbound MFA trust)

| Setting | Value |
|---|---|
| Location | Entra ID -> External ID -> Cross-tenant access settings |
| Tab | Organizational settings (per partner) or Default settings |
| Path | Inbound access -> Trust settings -> Customize settings |
| Option | Trust multifactor authentication from Microsoft Entra tenants |
| State | Ticked (on an organizational entry if you have a partner tenant, otherwise just locate it under Default settings) |

<img width="306" height="463" alt="image" src="https://github.com/user-attachments/assets/0a3c0120-b778-475e-9cac-1400af87105d" />


<img width="682" height="213" alt="image" src="https://github.com/user-attachments/assets/8604b167-faa5-4569-a242-12d9e60e2f89" />










