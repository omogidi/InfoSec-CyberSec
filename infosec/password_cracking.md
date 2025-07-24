
# 🔐 INFO6001 - Lab Assignment 7: Password Cracking

This lab focuses on cracking Windows password hashes using **John the Ripper** and **L0phtCrack (LC6)**, as well as analyzing the effects of storing LAN Manager (LM) hashes on password security.

---

## 🧩 Part A – John the Ripper

### 🛠️ Environment Setup
- VM: Windows 7 (INFO6001 image)
- Created folder: `C:\pwd`
- Copied from `C:\security\PwdCrackers`:
  - `john-17`
  - `pwdump7`

### 👤 User Accounts Created

| Username   | Password Type            | Example        |
|------------|--------------------------|----------------|
| `user1`    | Simple, >7 characters    | `elephant`     |
| `user2`    | Simple, <7 characters    | `horse`        |
| `user3`    | 15+ characters           | `myscreencaptures` |
| `user4`    | Complex, 6 characters    | `Pa$$Me`       |
| `MyFOLID`  | Standard account         | `Fanshawe`     |

### 🔍 Hash Extraction & Cracking
- Extracted hashes:
  ```cmd
  pwdump7 > pwdcrack
  ```
- Moved `pwdcrack` to `C:\pwd\john-17\run`
- Ran John the Ripper:
  ```cmd
  john.exe pwdcrack
  ```
- Show cracked passwords:
  ```cmd
  john.exe --show pwdcrack
  ```
- View result file:
  ```cmd
  type john.pot
  ```

### 🧠 Notes
- Brute force command: `john.exe -i pwdcrack`
- Cracking long or complex passwords may take considerable time

---

## 🧩 Part B – L0phtCrack 6 (LC6)

### 🧰 Installation
- Downloaded: `lc6setup_V6.0.20.zip`
- Installed LC6 and WinPcap (if prompted)

### 🔐 Password Audit Steps
1. Run LC6
2. Wizard flow:
   - Retrieve from local machine
   - Strong Password Audit
   - Select all report styles
3. View results (LM and NTLM hashes)

### 🔧 Cracking Options
- Explored:
  ```
  Session → Session Options
  ```
  - Dictionary
  - Hybrid
  - Brute force
- Opened default dictionary:
  ```
  C:\Program Files\L0phtCrack 6\words-english.dic
  ```

---

## 🧩 Part C – Local Security Policy

### 🔒 Disable LM Hash Storage
1. Open:
   ```
   Control Panel → Administrative Tools → Local Security Policy
   ```
2. Navigate to:
   ```
   Local Policy → Security Options
   ```
3. Enable:
   ```
   Network Security: Do not store LAN Manager hash value on next password change
   ```

### 🔄 Password Change
- Changed `user1` password to: `laptop`
- Re-ran LC6 audit

### 📊 Observation
- With LM hashes disabled, cracking `laptop` was significantly harder compared to `elephant`
- Demonstrated impact of legacy hash storage on security

---

## 📸 Screenshots Included (in .ppt submission)
1. Username and password hashes (`pwdcrack`)
2. John the Ripper cracked results
3. LC6 cracked results
4. LC6 cracking options screen
5. Final audit with LM hash disabled

---

## ✅ Summary of Skills Learned

- Windows password hash extraction with `pwdump7`
- Cracking hashes using John the Ripper and LC6
- Brute force and dictionary cracking analysis
- Effect of LM hash storage on crackability
- Understanding and configuring local security policy in Windows

---
