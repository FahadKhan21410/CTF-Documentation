# TryHackMe - Meow (Telnet CTF)

## Challenge Info
* Platform: TryHackMe
* Challenge Name: Meow
* Category: Network (Telnet)
* Difficulty: Very Easy
* Status: Solved Theoretically (can't spawn machine, already solved)

## Task Description
* We have been given access to a machine which is already vulnerable, The task is to enumerate it(Gather as much detailed information as possible about the target system), identify open ports, and exploit them to gain access.

## Thought Process & Methodology
## Enumeration

### Step 1: Nmap Scan

* Firstly we are going to use `nmap` to find which port is open
    `nmap -sC -sV <target-ip>`
  * -sC is just we telling nmap to use your own default script like service detection, banner grabbing etc.
  * -sV is to detect the version of ports
  * `nmap -A <target-ip>` -> -sC + -sV = -A (Agressive Scan)
  * Result:
    ```bash
    PORT   STATE SERVICE VERSION
    23/tcp open  telnet
  * This shows that Telnet (port 23) is open.
  
### Step 2: Connection

* To connect with the telnet port we use `telnet <target-ip>`
* After you are connected, you'll be asked for a username we will be using `root` because it doesnt ask for password (until it is explicitly defined)
* Result:
  ```bash
  Trying <target-ip>...
  Connected to <target-ip>.
  Escape character is '^]'.
  login: root

### Exploring The System

* Once you are logged you'll see something like this `root@<machine>`
* Now we can access directories by using `cd`(Change Directory)
* To view contents of the directory `ls`(list) is used
  ```bash
  root@<machine>:~# cd /
  root@<machine>:/# ls
  bin  boot  dev  etc  home  root  tmp  usr  var
* Most importantly, in a CTF we would look for the flag
  ```bash
  root@<machine>:~# cat /root/flag.txt
  THM{c0ngr4ts_y0u_own3d_t3ln3t}

## What is Telnet?
* An old network protocol which allows users to access computer remotely using command line interface (CLI)
* It is insecure because it transmits data (including credentials) in plaintext
* The modern replacement for Telnet (port 23) is SSH (port 22).

## TakeAway
* Importance of enumeration before exploitation.
* How to look for open ports
* Learned how to connect to services running on legacy(Old) or unusual ports
* Understood why Telnet is insecure and why SSH is preferred.
* Reinforced habit of looking for flags or sensitive files once inside.

## Note
* This is a practice scenario. Machines on platforms like TryHackMe are intentionally vulnerable
* In real-world situations, gaining root access this easily is highly unlikely.
> If you enumerate properly, the exploit usually reveals itself 
  
## Disclaimer

You may use this Knowledge:

* To learn security concepts
* For testing in controlled environments with explicit permission
* As part of ethical hacking or cybersecurity research

You may not use this Knowledge:

* For unauthorized access to systems
* To harm, disrupt, or compromise any network, server, or individual

By reading this write-up, you agree that any and all responsibility lies with you, the user.
