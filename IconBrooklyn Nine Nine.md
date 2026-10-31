
## FTP Anonymous Access, SSH Brute Force, and Privilege Escalation

### 1. Introduction

Brooklyn Nine Nine is an easy-level Linux CTF room on TryHackMe designed to improve practical penetration-testing skills. The challenge involves network scanning, FTP enumeration, SSH access, password discovery, and Linux privilege escalation.
### 2. Lab Objectives

- Check target machine reachability.
- Identify open ports and running services.
- Investigate anonymous FTP access.
- Discover information from `note_to_jake.txt`.
- Obtain SSH credentials for the Jake user.
- Enumerate user directories and locate the first flag.
- Review sudo permissions and verify root flag access.
- Collect both flags.

---
## Step 1: Check Target Reachability Using Ping

The first step was to verify whether the target lab machine was reachable over the network.

**Command:**

bash

```
ping 10.49.147.147
```

**Observation:**

The target machine's network reachability was checked successfully.
![[{FD4F7CEC-0C66-4355-8D4C-1164F1434F26}.png]]
## Step 2: Port Scanning Using Nmap

After checking network connectivity, an Nmap scan was performed to identify open ports and running services.

**Command:**

bash

```
nmap -A 10.49.147.147
```

**Observation:**
Three services were identified

| Port | Service | Status |
| ---- | ------- | ------ |
| 21   | FTP     | Open   |
| 22   | SSH     | Open   |
| 80   | HTTP    | Open   |
![[{2F28B8AC-C36A-4544-BB7B-E1371C881B78}.png]]
## Step 3: Anonymous FTP Login and File Discovery

An attempt was made to access the FTP service using anonymous authentication.

**Command:**

bash

```
ftp 10.49.147.147
```

**Login credentials:**

- Username: **anonymous**
- Password: **empty**

The anonymous FTP login was successful.

During FTP enumeration found the **note_to_jake.tx**t file.
The file contained a message intended for the user Jake.
cat the note_to_jake.txt file than found the **Jake please change you passowrd**``.

![[{D9A8B79D-861C-4223-8BB3-10C1CA571C8C}.png]]
## Step 4: Initial SSH Login Attempt

Based on the information obtained from the FTP note, an SSH login attempt was made using the Jake username.

**Command:**

bash

```
ssh jake@10.49.147.147
```

An initial attempt was made without a password.
The passwordless SSH login attempt was **unsuccessful**.

![[{929FC032-61A9-4566-81E4-2D19E4850BD0}.png]]
## Step 5: SSH Password Discovery

A password-auditing attempt was performed against the Jake SSH account using **Hydra** in the authorized lab environment.

**Tool Used:** Hydra
**Service Tested:** SSH
**Target Username:** Jake
The password-auditing process identified the following credentials:

bash

```
hydra -l jake -P /usr/share/wordlist/rockyou.txt ssh://10.49.147.147
```

| Field    | Value         |
| -------- | ------------- |
| Username | **jake**      |
| Password | **987654321** |

![[{9AB3F3BD-4DEC-4097-B865-0242BC3C0BD0}.png]]
## Step 6: SSH Login and User Flag Discovery

After obtaining the credentials, SSH access was established using the Jake account.

**Command:**

bash

```
ssh jake@10.49.147.147
```
### 6.1 Holt Directory Investigation

The `holt` directory was selected for further investigation.

The following files were identified:

text

```
!cat user.txt
```

The `user.txt` file contained the first flag.

**found the User Flag from the user.txt**. 
### 6.2 Sudo Permission Enumeration

Next, the sudo permissions assigned to the Jake user were reviewed.

**Command:**

bash

```
sudo -l
```

The output was examined to identify the commands that could be executed with elevated privileges.
The sudo permissions provided information about the user's available administrative privileges.
### 6.3 Root Flag Discovery

The root flag was located at:

text

```
less /root/root.txt
```

The file was inspected using the permitted `less` command after obtaining authorized access.

**Found the Root Flag in root.txt.**

![[{EC04469F-F289-41A5-A2DC-CE2F2CCAB4C4}.png]]

![[{7535F7E6-09F0-4F18-B0DD-E7C3FA2740ED} 1.png]]
## Step 7: Flag Collection

During the assessment, two flags were identified.

|Flag|Location|Status|
|---|---|---|
|User Flag|`/home/holt/user.txt`|Retrieved|
|Root Flag|`/root/root.txt`|Retrieved|

**Combined Flag Results:**

text

```
User Flag1 : ee11cbb19052e40b07aac0ca060c23ee
Root Flag2 : 63a9f0ea7bb98050796b649e85481845
```

---
## 8. Final Assessment Summary

|Step|Activity|Result|
|---|---|---|
|1|Ping connectivity check|Completed|
|2|Nmap service enumeration|FTP, SSH, HTTP identified|
|3|Anonymous FTP authentication|Successful|
|4|Initial SSH authentication|Unsuccessful|
|5|SSH password auditing|Password discovered|
|6|Jake SSH access and filesystem investigation|Successful|
|7|User and root flag collection|Two flags identified|
## 9. Security Findings

The assessment identified the following security weaknesses:

1. **Anonymous FTP Access:** Allowed users to access FTP files without authentication.
2. **Information Disclosure:** The `note_to_jake.txt` file exposed useful account information.
3. **Weak SSH Password:** The Jake account used a weak password.
4. **Sudo Misconfiguration:** Sudo permissions required review for excessive privileges.
5. **Unauthorized File Access Risk:** Root-level file access was possible after privilege escalation.
## 10. Recommendations

- Disable anonymous FTP access if not required.
- Restrict access to sensitive files.
- Never store passwords in publicly accessible files.
- Use strong and unique SSH passwords.
- Prefer SSH key-based authentication.
- Enable login rate limiting to prevent repeated password attempts.
- Apply the principle of least privilege to sudo permissions.
- Restrict access to root-owned files.
## 11. Conclusion

The TryHackMe Brooklyn Nine Nine lab was successfully completed. The assessment included network scanning, FTP enumeration, SSH access, and privilege escalation. Both user and root flags were identified, demonstrating successful access at both levels.

**Final Outcome:** Successfully completed the lab and verified both flags.

---
## Lab Information

- **Report Type:** TryHackMe – Brooklyn Nine Nine
- **Environment:** Authorized Practice Lab / CTF
- **Tools Used:** Ping, Nmap, FTP, SSH, Hydra, sudo, less
- **Assessment Status:** Completed (pending insertion of verified flag values).

