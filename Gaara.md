# OffSec Proving Grounds – Gaara Writeup

## Machine Information

|Category|Details|
|---|---|
|Platform|[OffSec Proving Grounds](https://www.offsec.com/products/proving-grounds/?utm_source=chatgpt.com)|
|Machine Name|Gaara|
|Difficulty|Easy|
|Operating System|Linux|
|Skills Learned|Enumeration, Steganography, Brute Force, Privilege Escalation, SUID Exploitation|

---

# Objective

The objective of this lab was to:

- gain initial access to the target machine
- enumerate the system
- exploit vulnerabilities
- escalate privileges to root
- capture both user and root flags

---

# Step 1 — Start the Lab

The Gaara machine was started from the OffSec Proving Grounds environment.

After deployment, the target IP address was provided.

---

# Step 2 — Port Scanning with Nmap

The first step was enumeration using Nmap.

Command used:

```
nmap TARGET_IP
```


## Result

The scan revealed two open ports:

|Port|Service|
|---|---|
|22|SSH|
|80|HTTP|

This indicated:

- SSH access might be possible later
- the web application required further investigation

---

# Step 3 — Web Enumeration & Steganography Analysis

The target IP was opened in the browser.

The webpage contained:

- a simple interface
- an image named:

```
gaara.jpg
```

## Downloading the Image

The image was downloaded locally for analysis.

### Initial Checks

I checked whether the image contained hidden data using steganography techniques.

Tools and methods used:

- `strings`
- `steghide`
- `CyberChef`
- metadata inspection

Example:

```
strings gaara.jpg
```

During the analysis, an ID-like encoded string was discovered inside the image.

The encoded data was then decrypted using:

- [CyberChef](https://gchq.github.io/CyberChef/?utm_source=chatgpt.com)
- Base64 decoding techniques

However, the decoded information turned out to be:

# a rabbit hole

This intentionally misleading clue consumed time and demonstrated an important lesson:

> not every discovered clue is useful.

---

# Step 4 — Brute Force Attack Using Hydra

Since SSH was available, I attempted credential attacks.

Tool used:

- Hydra

Command:

```
hydra -l gaara -P rockyou.txt ssh://TARGET_IP
```

## Explanation

|Option|Meaning|
|---|---|
|`-l`|Username|
|`-P`|Password wordlist|
|`ssh://`|Target protocol|

Eventually, Hydra successfully discovered valid credentials.

---

# Step 5 — SSH Login

Using the credentials obtained from Hydra, I logged into the target machine through SSH.

Command:

```
ssh gaara@TARGET_IP
```

This provided initial shell access.

---

# Step 6 — Capturing User Flag

After gaining access, I searched for important files and discovered:

- `flag.txt`
- `log.txt`

Initially, `flag.txt` appeared to be the target, but after investigation:

# the actual user flag was located inside `log.txt`

This completed approximately:

```
50% completed
```

This step again reinforced the importance of:

- careful enumeration
- reading files properly
- avoiding assumptions

---

# Step 7 — Privilege Escalation Enumeration

After obtaining user access, I began privilege escalation enumeration.

The command used:

```
find / -perm -u=s 2>/dev/null
```

## Purpose of the Command

This command searches the entire system for:

# SUID binaries

---

# Understanding SUID

SUID (Set User ID) means:

> programs run with the permissions of the file owner.

If the owner is:

```
root
```

then the program executes with root privileges.

Misconfigured SUID binaries can become:

# privilege escalation vectors

---

# Step 8 — Vulnerable SUID Binary Discovery

During enumeration, I discovered that:

```
gdb
```

had the SUID bit enabled.

This was a serious misconfiguration because:

- GDB allows command execution
- GDB supports Python execution
- SUID GDB can lead to root shell access

---

# Step 9 — Root Exploitation

To exploit the vulnerable SUID GDB binary, I used the following payload:

```
gdb -nx -ex 'python import os; os.execl("/bin/sh","sh","-p")' -ex quit
```

## Explanation

This payload:

- launches GDB
- executes embedded Python code
- spawns `/bin/sh`
- preserves root privileges using `-p`

---

# Successful Privilege Escalation

After execution:

```
id
```

Output:

```
uid=1000(gaara) euid=0(root)
```

This confirmed:

# root access obtained

---

# Step 10 — Capturing Root Flag

After privilege escalation, I navigated to the root directory and found:

```
root.txt
```

This file contained:

# Flag 2 (Root Flag)

The machine was successfully completed.

---

# Tools Used

|Tool|Purpose|
|---|---|
|Nmap|Port scanning & enumeration|
|Dirsearch|Directory enumeration|
|Gobuster|Hidden file discovery|
|Hydra|SSH brute forcing|
|[CyberChef](https://gchq.github.io/CyberChef/?utm_source=chatgpt.com)|Data decoding|
|Base64|Decoding encoded strings|

---

# Important Concepts Learned from Gaara

---

## 1. Enumeration is Everything

Gaara strongly reinforces the idea:

> “If you’re stuck, enumerate more.”

Most failures happen because enumeration stops too early.

---

## 2. Rabbit Holes Exist

The machine intentionally includes:

- fake credentials
- misleading clues
- unnecessary information

This mimics:

# real-world penetration testing scenarios

---

## 3. Small Clues Matter

A single hidden encoded string eventually helped guide the attack path.

This taught:

- patience
- observation
- attention to detail
- careful investigation

---

## 4. Privilege Escalation Basics

Gaara is an excellent beginner machine for understanding:

- SUID binaries
- GTFOBins concepts
- Linux privilege escalation
- misconfigured privileged programs

---

# Final Thoughts

The Gaara lab was an excellent beginner-friendly machine that focused heavily on:

- methodology
- enumeration
- patience
- privilege escalation fundamentals

This lab improved my understanding of:

- Linux privilege escalation
- brute force attacks
- steganography analysis
- rabbit holes in penetration testing
- SUID exploitation using GTFOBins

The machine demonstrated that successful penetration testing depends more on:

# careful enumeration and persistence

than advanced exploitation techniques.