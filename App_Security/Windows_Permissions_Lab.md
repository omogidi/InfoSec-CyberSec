# Lab-04B: Windows Permissions and Access Control

This lab guides you through exploring and managing NTFS permissions, Access Control Lists (ACLs), and file sharing security in a Windows environment.

---

## Requirements

* Server 2008 R2 VM (2008-FOLusername)
* Windows 7 VM (W7-FOLusername)
* Windows 10 VM (Win10-FOLusername)
* Domain Administrator Credentials:

  * Server 2008: `Windows12`
  * Windows 7/10: `Windows1`

See how to set_up these servers here
---

## Part 1: NTFS Permissions (Windows 7 VM)

### Create Folders and Files

1. Log in as `User-Limited`.
2. Create the folder `C:\INFO6003`.
3. Inside, create:

   * File: `File1.txt`
   * Subfolder: `Sub1`
4. Enable file extensions in Folder Options.

### Deny Administrator Access

1. Right-click `C:\INFO6003` > Properties > Security > Advanced > Change Permissions.
2. Select `Administrators` > Edit > Deny `Full Control`.
3. Confirm all warnings.

### Check Inherited Permissions

1. View `Sub1` security.
2. Note inherited deny from `INFO6003`, even if other full control permissions exist.

### Override Inheritance for `Sub1`

1. Log in as `User-Limited`.
2. Open `Sub1` > Properties > Security > Advanced > Change Permissions.
3. Uncheck "Include inheritable permissions..." and remove inherited entries.
4. Add `User-Admin` and `User-Limited` with full control.

### Access Verification

1. Log in as `User-Admin`. Try accessing `C:\INFO6003` (access denied).
2. Use Command Prompt:

   ```bash
   cd C:\INFO6003\Sub1
   ```

   Confirm whether access is allowed or denied.

---

## Part 2: Group Membership Conflict

1. As `User-Limited`, open **Computer Management**.
2. View `Administrators` and `Users` group memberships.

   * `User-Admin` is a member of both.
3. Create folder `C:\Secure1`.
4. Deny `Users` group full control.
5. Log in as `User-Admin` and attempt access to `Secure1` (access denied due to explicit deny).

---

## Part 3: Access Control Lists (ACLs) with `icacls`

### View Permissions

```bash
cd C:\INFO6003
icacls File1.txt
icacls Sub1
```

* Note:

  * `CI` = Container Inherit
  * `OI` = Object Inherit
  * `F` = Full Control

### Deny Permissions

```bash
icacls File1.txt /deny Users:F
icacls File1.txt /inheritance:r
```

* `(N)` = No access (explicit deny)

### Canonical Order

Run:

```bash
icacls /?
```

* Review canonical ACE order: deny entries are evaluated before allow entries.

---

## Part 4: Registry Permissions

1. Log in as `User-Admin`.
2. Run `regedit`.
3. Navigate to `HKEY_LOCAL_MACHINE\SAM\SAM`.
4. Only `SYSTEM` has full control. `Administrators` have limited access.

> Note: Even Administrators are restricted from sensitive registry areas for security reasons.

---

## Part 5: Run As Another User

1. Open CMD:

   ```bash
   whoami
   cd C:\INFO6003
   runas /user:User-Limited cmd
   ```
2. In the new window:

   ```bash
   cd C:\INFO6003
   icacls File1.txt
   icacls Sub1
   whoami
   ```

---

## Part 6: File Sharing and NTFS Permissions (Server 2008)

1. From Windows 10 VM, log in as Domain Admin.
2. On Server 2008, create `C:\Data` folder.
3. Share it with **Full Control**.
4. Set NTFS permissions:

   * `User-Limited`: Read-only
   * `User-Admin`: Read/Write

> Effective permissions = most restrictive between Share and NTFS.

---

---

**End of Lab**
