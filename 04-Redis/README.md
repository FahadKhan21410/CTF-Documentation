# HackTheBox - Redeemer (Redis CTF)

## Challenge Info

* Platform: HackTheBox
* Challenge Name: Redeemer
* Category: Network (TCP)
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
    6379/tcp open redis    
  * TCP port 6379 is commonly used for redis 

### Step 2: Connection

* To connect/interact with redis(Remote Directory Server) we need to install its Cli `sudo apt install redis-tools`
* After installing redis tools we can use `redis-cli help` to see how to connect with the host we are targeting 
* The command to connect with a host is `-h <hostname>` : specify the hostname of the target to connect to
* Result:
  ```bash
   redis-cli  -h <target-ip>
   <targert-ip:port>
* You can do `DBSIZE` to find out how tables are in the redis (Database)
* For our example we only have 1 in the array so using `<targert-ip:port> select 0`
* Each piece of data in Redis is associated with a unique key, which acts as its address or name. So, by doing `keys *` We can list all the key available
* Result
  ```bash
  <targert-ip:port> keys *
  1) "numb"
  2) "stor"
  3) "temp"
  4) "flag"
* Finally, we can view the values stored for a corresponding key using the `get` command
* `get flag`
* Result: HTB{c0ngr4ts_y0u_own3d_R3d15}

## What is Redis?
* Redis (REmote DIctionary Server) is an open source, in-memory, NoSQL("not only SQL", refers to a class of non-relational databases designed for storing and retrieving data in ways that differ from traditional relational databases)
  key/value store that is used primarily as an application cache or quick-response database.
* Redis stores data in memory, rather than on a disk or solid-state drive (SSD), which helps deliver unparalleled speed, reliability, and performance.

## TakeAway
* Learned to use nmap to identify open ports and detect Redis service on TCP 6379
* Practiced connecting to Redis using redis-cli and exploring available databases
* Enumerated keys with keys * and retrieved data using the get command
* Understood how insecure Redis instances (without authentication) can expose sensitive information
* Gained a understanding of Redis as an in-memory key-value store and its security risks if misconfigured

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
