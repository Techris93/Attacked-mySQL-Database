# Attacked-mySQL-Database
## Objective
- View and Analyze a PCAP file from a previous attack against a SQL database. <br>
Part 1: Open Wireshark and load the PCAP file. <br>
Part 2: View the SQL Injection Attack. <br>
Part 3: The SQL Injection Attack continues… <br>
Part 4: The SQL Injection Attack provides system information. <br>
Part 5: The SQL Injection Attack and Table Information <br>
Part 6: The SQL Injection Attack Concludes.
## Required Resources
- CyberOps Workstation Virtual Machine
- Internet access

## Instructions
I will use Wireshark, a common network packet analyzer, to analyze network traffic. After starting
Wireshark, I will open a previously saved network capture and view a step-by-step SQL injection attack
against a SQL database. 
<br>
#### Part 1: Open Wireshark and load the PCAP file.
- The PCAP file opens within Wireshark and displays the captured network traffic. This capture file extends
over an 8-minute (441 seconds) period, the duration of this SQL injection attack.
<br><img src="https://i.imgur.com/Zc19AxV.png" height="80%" width="80%" alt="unfiltered pcap"/><br>
#### Part 2: View the SQL Injection Attack.
- Within the Wireshark capture, right-click line 13 and select Follow > HTTP Stream. Line 13 was chosen because it is a GET HTTP request.
  <br><img src="https://i.imgur.com/t1mYTJd.png" height="80%" width="80%" alt="HTTP Stream1 pcap"/><br>
- The source traffic is shown in red. The source has sent a GET request to host 10.0.2.15. In blue, the destination device is responding back to the source
- In the Find field, enter 1=1. Click Find Next.
<br><img src="https://i.imgur.com/IviGT1N.png" height="80%" width="80%" alt=" filtered HTTP Stream1 pcap"/><br>
- The attacker has entered a query (1=1) into a UserID search box on the target 10.0.2.15 to see if the
application is vulnerable to SQL injection. Instead of the application responding with a login failure message, it responded with a record from a database.
#### Part 3: The SQL Injection Attack continues...
- Within the Wireshark capture, right-click line 19 and select Follow > HTTP Stream.
<br><img src="https://i.imgur.com/R2qbgkZ.png" height="80%" width="80%" alt="filtered HTTP Stream1 pcap2"/><br>
- The attacker has entered a query (1’ or 1=1 union select database(), user()#) into a UserID search box on the target 10.0.2.15. Instead of the application responding with a login failure message, it responded with the information above.
- The database name is dvwa and the database user is root@localhost. There are also multiple user accounts being displayed.
#### Part 4: The SQL Injection Attack provides system information.
- Within the Wireshark capture, right-click line 22 and select Follow > HTTP Stream.
<br><img src="https://i.imgur.com/vS6GNa5.png" height="80%" width="80%" alt="filtered HTTP Stream1 pcap3"/><br>
- The attacker has entered a query (1’ or 1=1 union select null, version ()#) into a UserID search box on the target 10.0.2.15 to locate the version identifier.
#### Part 5: The SQL Injection Attack and Table Information. 
- Within the Wireshark capture, right-click line 25 and select Follow > HTTP Stream.
<br><img src="https://i.imgur.com/BlfyQ21.png" height="80%" width="80%" alt="filtered HTTP Stream1 pcap4"/><br>
- The attacker has entered a query (1’or 1=1 union select null, table_name from
information_schema.tables#) into a UserID search box on the target 10.0.2.15 to view all the tables in the database
#### Part 6: The SQL Injection Attack Concludes.
- Within the Wireshark capture, right-click line 28 and select Follow > HTTP Stream.
<br><img src="https://i.imgur.com/NARkrIZ.png" height="80%" width="80%" alt="filtered HTTP Stream1 pcap5"/><br>
- The attacker has entered a query (1’or 1=1 union select user, password from users#) into a UserID search box on the target 10.0.2.15 to pull usernames and password hashes!
- Using a website such as https://crackstation.net/, we can crack the non-salted hashes to get the plain-text password
<br><img src="https://i.imgur.com/ugorkFQ.png" height="80%" width="80%" alt=" cracked hashes"/><br>
## Conclusion
Websites are commonly database-driven and use the SQL language. They can be prevented by using but not limited to the following ways:
- Filter user input
- Deploy a web application firewall
- Disable unnecessary database features/capabilities
- Monitor SQL statements
- Use parameters with stored procedures
- Use parameters with dynamic SQL

    
  
