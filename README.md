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
### Project 3 Deployment Summary: Administration Portal Access Lockdown

**Problem Statement:**
The default baseline properties configuration within the Microsoft Entra ID (MEID) tenant allowed standard, non-administrative user accounts to successfully log into the administrative management interface, exposing tenant configuration metrics, directory object lists, and schema metadata to unauthorized entities.

**Root Cause Analysis:**
The platform architecture implements an open directory read-access policy by default. Unless the administrative portal restriction constraint is explicitly activated within the global directory schema, the web interface honors standard user session tokens for portal entry, disabling write permissions but leaving directory structure data visible.

**Remedy and Final State:**
The Global Administrator logged into the portal, navigated to the Microsoft Entra ID (MEID) User settings workspace, flipped the "Restrict access to Microsoft Entra admin center" configuration toggle from "No" to "Yes", and saved the modification. After allowing a 15-minute propagation window for the global policy change to replicate across all distributed identity endpoints, validation testing in a fresh private browser window confirmed the successful enforcement of three distinct, role-bounded architectural states:

*   **Global Administrator:** Retains full authorized Read and Write capabilities to view and alter global tenant configurations.
*   **User Administrator (UA):** Retains authorized Read capabilities to navigate the directory but is restricted from modifying the underlying tenant-wide properties.
*   **Bravo Engineer (Standard User):** Encounters an immediate, hard Access Denied block page returning an explicit insufficient privileges error, completely blocking entry to the administrative management perimeter.
