### Project 3 Deployment Documentation: Administration Portal Access Lockdown

**Project Objective:**
To harden the baseline tenant perimeter by restricting access to the administrative interface, ensuring that only security principals with explicit directory roles can view tenant configurations and directory data.

**Initial State:**
The tenant-wide property for portal restriction is set to its default value ("No"). This allows standard, non-administrative user accounts to browse the directory structure, view other user objects, and inspect configuration metadata via the administrative user interface.

**Configuration Procedure:**
1. Sign into the Microsoft Entra admin center using an account with Global Administrator privileges.
2. Navigate to **Microsoft Entra ID (MEID) > Identity > Overview > Properties**.
3. Locate the configuration toggle labeled **Restrict access to Microsoft Entra administration portal**.
4. Change the toggle state from **No** to **Yes**.
5. Click **Save** at the top of the properties blade to write the new policy constraint to the directory database.

**Verification Matrix:**

| Security Principal | Identity State / Directory Role | Expected Testing Outcome | Actual Testing Outcome |
|---|---|---|---|
| **Alpha Engineer** | User Administrator (UA) (Active Privileged Identity Management (PIM) Session) | **Success:** Full authorized access to the administrative portal interface. | Pending |
| **Bravo Engineer** | Standard User Account (Zero Administrative Directory Roles) | **Failure:** Hard access block page returning an explicit insufficient privileges error. | Pending |

**Validation Steps:**
1. Open a completely fresh private browser window to ensure no legacy session tokens or browser cookies interfere with the authorization handshake.
2. Authenticate as **Bravo Engineer (Standard User)** and attempt to navigate directly to the administrative portal. Confirm that the platform prevents entry and enforces the perimeter lockdown.
3. Close the private browser window entirely to terminate the session.
4. Open a new private browser window, authenticate as **Alpha Engineer**, ensure the User Administrator (UA) role is actively elevated through the Privileged Identity Management (PIM) tool, and verify that administrative data remains fully accessible.
