# metasploit-basics

## search

```bash
msfconsole

show all

show auxiliary

show exploits

search ABCD
search type:exploit
search platform:Windows
search name:CVExxxx
search exploit/windows/smb


search cve:2009 type:exploit
search cve:2009 type:exploit platform:-linux
search cve:2009 -s name
search type:exploit -s type -r

# we can also request info on the exploit
info exploit/windows/smb/ms17_010_eternalblue
```

## sample with search over multiple types

```bash
msf5 > search type:-auxiliary type:-post type:-payload type:-exploit type:-encoder type:-evasion type:-nop
[-] No results from search
msf5 > 


## "play" with the `-` 

msf5 > search type:-auxiliary type:-post type:-payload type:exploit type:-encoder type:-evasion type:-nop platform:windows check:yes

Matching Modules
================

   #    Name                                                                   Disclosure Date  Rank       Check  Description
   -    ----                                                                   ---------------  ----       -----  -----------
   0    exploit/multi/http/ajaxplorer_checkinstall_exec                        2010-04-04       excellent  Yes    AjaXplorer checkInstall.php Remote Command Execution
   1    exploit/multi/http/coldfusion_rds_auth_bypass                          2013-08-08       great      Yes    Adobe ColdFusion RDS Authentication Bypass
(...)


```

----

## use

```bash
msf > use exploit/windows/smb/ms17_010_eternalblue

show options

# if we want to go back to the search
msf > back
```
