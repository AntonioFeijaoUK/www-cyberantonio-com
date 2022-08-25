metasploid discovery module

msf > `use auxiliary/scanner/discovery/arp_sweep`

`search portscan`

`auxiliary/scanner/portscan/syn`

`show options`

db nmap

`service postgresql start`

`msfdb init`

msf > `db_status`

msf > `db_nmap -sV [Ip Address] -Pn`

msf > `db_nmap -sV 10.102xxxx -p 22,8081`

msf > `db_nmap -sV 10.102.1.223 -p 1,10000`
