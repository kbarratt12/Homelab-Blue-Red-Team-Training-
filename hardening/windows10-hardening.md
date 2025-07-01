# Windows 10 Basic System Hardening Project

**Date Completed:** April 25, 2025  
**Objective:**  
Harden a Windows 10 virtual machine by applying basic security controls in alignment with Security+ best practices.

---

## Step 1: Guest Account Check

**Before:**  
- No Guest account appeared in user listings.
![image](https://github.com/user-attachments/assets/e942efe7-0922-4846-ac8a-f0e1b6732379)


**After:**  
- No action taken. No Guest account was present by default.

**Notes:**  
- Confirmed that the Guest account was already disabled or not visible.
- No changes were required.

---

## Step 2: Create Strong Admin Account and Disable Default Administrator

**Before:**  
- Only the built-in `Administrator` account existed.
![image](https://github.com/user-attachments/assets/f4b47a23-56f5-47f8-924d-392641ca2f98)


**After:**  
- Created a new administrator account with a unique name and strong password.
- Disabled the default built-in `Administrator` account.

![image](https://github.com/user-attachments/assets/3989004b-2399-47cc-b970-7b3a9714b224)


**Purpose:**  
To reduce exposure to brute-force or privilege escalation attacks that target default accounts.

---

## Step 3: Enforce Password Complexity

**Before:**  
- Attempted to configure password policies via `secpol.msc`, but Windows 10 Home Edition does not include Local Security Policy tools.

**After:**  
- Manually set strong passwords when creating user accounts:
  - Minimum 14 characters
  - Includes uppercase, lowercase, numbers, and special characters

**Notes:**  
- Complexity was enforced manually at account creation.
- Recommended: Use Windows 10 Pro or centralized management (e.g., Group Policy) for full password policy enforcement.

---

## Step 4: Enable Windows Firewall and Defender Antivirus

**Windows Firewall**

**Before:**  
- Navigated to Control Panel → System and Security → Windows Defender Firewall.
- Firewall was enabled for Domain, Private, and Public profiles.

**After:**  
- No changes were required. Firewall remained active.

![image](https://github.com/user-attachments/assets/0ab86b80-1c1f-4902-a818-203e598d431f)

**Windows Defender Antivirus**

**Before:**  
- Navigated to Settings → Update & Security → Windows Security → Virus & Threat Protection.
- Real-time and cloud-based protection were already enabled.

**After:**  
- No changes were required.

![image](https://github.com/user-attachments/assets/eee835ad-98c3-470a-97f3-8eb65b3f54fa)


---

## Step 5: Remove Unnecessary Applications (Bloatware)

**Before:**  
- System included multiple pre-installed apps (e.g., Xbox Console Companion, Candy Crush, Groove Music, Cortana, Paint 3D, etc.)

![image](https://github.com/user-attachments/assets/cb18929a-3a43-4aa1-8532-09dcdd797199)


**After:**  
- Uninstalled unnecessary apps via Settings → Apps → Apps & Features, including:
  - Xbox Game Bar, Candy Crush, 3D Viewer, Groove Music, Movies & TV, Skype, Cortana, Mixed Reality Portal, Tips, Weather, OneNote, Solitaire Collection, Paint 3D, Voice Recorder

![image](https://github.com/user-attachments/assets/bcc68d86-40c9-46e1-9e29-ed6bd77c8d5c)


**Purpose:**  
Reduce the attack surface, free system resources, and streamline the VM.

---

## Final Notes

- Screenshots were captured for each major change.
- System was hardened based on Security+ recommended practices.
- Suggested additional hardening steps:
  - Enable BitLocker Drive Encryption for data-at-rest protection
  - Ensure automatic Windows Updates are configured and enforced
  - Create a system restore point or VM snapshot after hardening
