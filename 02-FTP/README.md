# HackTheBox - Fawn (FTP CTF)

## Challenge Info

* Platform: HackTheBox
* Challenge Name: Fawn
* Category: Network (FTP)
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
    21/tcp open  ftp     vsftpd 3.0.3
  * This shows that ftp (port 21) is open.

### Step 2: Connection

* To connect with the ftp port we use `ftp <target-ip>`
* After you are connected, you'll be asked for a username we will be using `anonymous` because you can login without having an account
* Result:
  ```bash
  ftp 10.129.117.38
  Connected to 10.129.117.38.
  220 (vsFTPd 3.0.3)
  Name (10.129.117.38:kali): anonymous
  331 Please specify the password.
  Password: -> (No Passowrd Required)
  230 Login successful.
  Remote system type is UNIX.
  Using binary mode to transfer files.
  ftp>

### Exploring The System

* Once you are logged you'll see something like this `ftp>`
* Now we can access directories by using `cd`(Change Directory)
* To view contents of the directory `ls`(list) is used
  ```bash
  ftp> ls
  229 Entering Extended Passive Mode (|||13761|)
  150 Here comes the directory listing.
  -rw-r--r--    1 0        0              32 Aug 30  2025 flag.txt
  226 Directory send OK.
  ftp>
   
* Most importantly, in a CTF we would look for the flag
* To download files from an ftp server you need to use `get` command
  ```bash
   ftp> get flag.txt
  local: flag.txt remote: flag.txt
  229 Entering Extended Passive Mode (|||20395|)
  150 Opening BINARY mode data connection for flag.txt (32 bytes).
  100% |**************************************************|    32      146.71 KiB/s    00:00 ETA
  226 Transfer complete.
  32 bytes received in 00:00 (0.45 KiB/s)
  ftp>
  ┌──(root㉿recognized)-[/home/recognized]
  └─# cat flag.txt 
  THM{c0ngr4ts_y0u_own3d_ftp}
  
## What is FTP?
* FTP is a standard network protocol that uses a client-server model (client "request" and server "provides") for transferring files between computers over TCP/IP networks.
* It relies on separate control and data connections, allowing a user to upload (put) or download (get) files to or from an FTP server.
* While useful, FTP is insecure as it transfers data in plain text, making SFTP (Secure File Transfer Protocol) modern alternative for secure data transfer.

## TakeAway
* Learned to use nmap with -sC, -sV, and -A for detailed enumeration
* Discovered that FTP (port 21) was open and vulnerable to anonymous login
* Practiced connecting to an FTP server and navigating directories (cd, ls)
* Retrieved files using the get command in FTP
* Found and read the flag.txt file successfully
* Understood why FTP is insecure (plain-text communication, no encryption) and why SFTP/SSH is the preferred secure alternative

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
