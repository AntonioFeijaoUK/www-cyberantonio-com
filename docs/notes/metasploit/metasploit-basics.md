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

----

## use

```bash
msf > use exploit/windows/smb/ms17_010_eternalblue

show options

# if we want to go back to the search
msf > back

```
