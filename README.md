# Active Directory Account Lockout & Password Reset

A hands-on homelab simulation covering end-to-end Tier 1/Tier 2 identity support: handling a user-reported laptop login issue and account lockout in **Active Directory (`ApexTech.com`)**, enforcing a mandatory password change on next logon, verifying workstation sign-in on **Windows 11 Enterprise**, and documenting the full lifecycle in **GLPI**.

## Lab Environment

* **Domain & Directory:** Active Directory Domain Services (`ApexTech.com`) on Windows Server (Domain Controller IP ending in `.22`)
* **Target User Account:** `goodmans` (`Goodman Shane`, location: `Dallas HQ - DFW-HQ`)
* **Client Workstation:** Domain-joined Windows 11 Enterprise
* **ITSM Platform:** Self-hosted GLPI (`Access & Identity > Password Reset / MFA`, Ticket `#52`)

## Incident Summary

* **Ticket:** `Password reset (52)` — classified as an **Incident** (`Access & Identity > Password Reset / MFA`)
* **Timeline:** Opened `2026-11-18 19:48:52`, resolved & closed `2026-11-18 20:22:56`, logged `0h10` active duration.
* **Outcome:** Account unlocked, temporary password issued out-of-band with **User must change password at next logon** enforced, user sign-in verified on Windows 11, and solution approved/closed in GLPI.

## Step-by-Step Walkthrough

### 1. Triage & Ticket Review (GLPI)
* Reviewed Ticket `#52` submitted by `Goodman Shane` (`I forgot the login to my laptop can you please reset it for me`).
* Assigned to `glpi` / `Tier 1 - Service Desk` under category `Access & Identity > Password Reset / MFA`.

### 2. Account Unlock & Password Reset (ADUC)
* Opened **Active Directory Users and Computers (ADUC)** for `ApexTech.com` and inspected the `goodmans` user properties.
* Checked **Unlock account** to clear the active lockout state on the Domain Controller.
* Performed a password reset with a unique temporary password and enabled **User must change password at next logon**.
* Delivered the temporary password out-of-band (via text message) rather than in plaintext inside the ticket thread.

### 3. Workstation Verification (Windows 11)
* Signed into the domain-joined Windows 11 Enterprise workstation using the temporary credential, completed the forced password change prompt, and confirmed full desktop access.

### 4. Resolution & Closure (GLPI)
* Added a helpdesk follow-up and formal resolution note documenting the AD unlock, out-of-band temporary password delivery, and forced password change[cite: 1].
* Once the user confirmed access and approved the solution, the ticket transitioned to **Closed** (`2026-11-18 20:22:56`).

## Key Takeaways & Troubleshooting Notes

* **AD Lockout Checkbox Behavior:** In ADUC's **Account** tab, the **Unlock account** checkbox is unchecked by default; checking it and applying clears the lockout flag when the account is locked.
* **Out-of-Band Credential Hygiene:** Temporary passwords should be unique per incident, short-lived, paired with forced rotation at next logon, and delivered outside email/ticket notes.
* **ITSM Metrics & Triage:** Deliberately aligning **Impact**, **Urgency**, and **Total duration** (`0h10`) helps keep SLA queues and reporting accurate.

## 📸 Lab Evidence

### 1. Initial Ticket in GLPI
Ticket `#52` (`Password reset`) reviewed in GLPI under `Access & Identity > Password Reset / MFA` prior to resolution.
![Unresolved Ticket](Unresolved%20Ticket.png)

### 2. Workstation Sign-In Lockout Symptom
Windows 11 login screen showing failed/delayed authentication attempts during the lockout scenario for `Shane R. Goodman`.
![Account Lockout Screen](Account%20lock%20out.png)

### 3. Locked Account State in Active Directory
Account lockout state verified on the `goodmans` user object in Active Directory Users and Computers (ADUC).
![Locked User Account](Locked%20user%20account.png)

### 4. Account Unlock & Password Reset
Account unlocked and temporary password configured with mandatory change at next logon in ADUC.
![Reset Password](Reset%20password.png)

### 5. Resolved & Closed Ticket in GLPI
Ticket `#52` updated with resolution notes, confirmed by the user, and closed after `0h10` of logged duration.
![Resolved Ticket](Resolved%20Ticket.png)
