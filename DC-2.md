# DC-2 OffSec Lab Writeup (50% Completion)

## Introduction

This writeup documents my progress on the DC-2 lab from the OffSec practice labs. The goal of this challenge was to perform reconnaissance, enumeration, credential discovery, and privilege escalation techniques in order to capture flags from the target machine.

At the current stage, I have completed approximately 50% of the lab.

---

# Lab Information

- Lab Name: DC-2
    
- Platform: OffSec Practice Lab
    
- Status: 50% Completed
    

---

# Step 1 – Start the Lab

Before beginning the assessment, I carefully read the lab description and objectives to understand the target environment and expected workflow.

This helped in identifying the possible attack surface and planning the enumeration process.

---

# Step 2 – Scan Open Ports Using Nmap

The first step after launching the target machine was network reconnaissance.

I used Nmap to identify the open ports and services running on the target.

```bash
nmap -A -p- <target-ip>
```

## Purpose

- Discover open ports
    
- Detect running services
    
- Identify service versions
    
- Gather useful enumeration information
    

During the scan, I identified that the HTTP service was running.

---

# Step 3 – Open the Target IP in the Browser

Since the HTTP port was open, I accessed the target IP address through the web browser.

```text
http://<target-ip>
```

The website loaded successfully and appeared to contain useful information for further enumeration.

---

# Step 4 – Configure the Hosts File

The website hinted at a domain name requirement. To properly resolve the domain locally, I edited the `/etc/hosts` file.

```bash
sudo nano /etc/hosts
```

Then I added the target IP and domain mapping.

Example:

```text
<target-ip>    dc-2
```

This allowed the website to function correctly using the hostname.

---

# Step 5 – Search the Website for Clues

After configuring the hosts file, I revisited the website and carefully inspected the pages for hidden clues.

During enumeration, I looked for:

- Hidden directories
    
- Keywords
    
- Usernames
    
- Hints in page source
    
- CMS information
    
- Robots.txt entries
    

The target appeared to be running WordPress.

---

# Step 6 – Create a Custom Wordlist Using CeWL

To generate a targeted password wordlist, I used CeWL.

```bash
cewl <url> -w wordlist.txt
```

## Purpose

CeWL crawls the website and extracts words that can later be used for password attacks.

The generated wordlist was saved as:

```text
wordlist.txt
```

---

# Step 7 – Enumerate Usernames Using WPScan

Since the target was using WordPress, I used WPScan to enumerate valid usernames.

```bash
wpscan --url <url> --enumerate u
```

## Result

Several valid usernames were identified from the WordPress installation.

---

# Step 8 – Save Usernames into a File

After collecting the usernames, I stored them inside a text file for brute-force testing.

```bash
sudo nano users.txt
```

The discovered usernames were added to:

```text
users.txt
```

---

# Step 9 – Brute Force Credentials Using WPScan

Next, I performed a password attack using the discovered usernames and the custom wordlist generated with CeWL.

```bash
wpscan --url <url> -U users.txt -P wordlist.txt
```

## Purpose

- Test usernames against potential passwords
    
- Discover valid login credentials
    

After some time, valid credentials were discovered.

---

# Step 10 – Valid Credentials Found

The brute-force attack successfully revealed a working username and password combination.

These credentials were later used for remote access.

---

# Step 11 – Login via SSH

Using the discovered credentials, I logged into the target machine through SSH.

```bash
ssh <username>@<target-ip>
```

After entering the password, shell access was obtained successfully.

---

# Step 12 – Search for the First Flag

Once inside the machine, I searched for text files that might contain flags.

Useful commands:

```bash
find / -name "*.txt" 2>/dev/null
```

I located the first flag successfully.

---

# Step 13 – Privilege Escalation and Flag 2

After obtaining initial access, I started privilege escalation enumeration.

I checked:

- Sudo permissions
    
- Writable files
    
- Misconfigured binaries
    
- User privileges
    
- PATH vulnerabilities
    
- Scheduled tasks
    

During the privilege escalation process, I successfully obtained elevated access and found the second flag.

---

# Current Progress

At this stage, I have completed approximately 50% of the DC-2 lab.

## Completed Tasks

- Reconnaissance
    
- Service enumeration
    
- WordPress enumeration
    
- Username discovery
    
- Password brute forcing
    
- SSH access
    
- Initial flag capture
    
- Partial privilege escalation
    

---

# Tools Used

- Nmap
    
- CeWL
    
- WPScan
    
- SSH
    
- Linux Enumeration Commands
    

---

# Skills Practiced

This lab helped improve practical skills in:

- Network reconnaissance
    
- Web enumeration
    
- WordPress security testing
    
- Password attacks
    
- Linux privilege escalation
    
- Flag discovery techniques
    

---

# Conclusion

The DC-2 lab provided hands-on experience in WordPress enumeration and Linux privilege escalation. Through systematic reconnaissance and enumeration, I was able to gain credentials, access the machine via SSH, and capture flags.

Although the lab is only 50% completed, it has already provided valuable practical cybersecurity experience and strengthened my understanding of penetration testing methodology.
