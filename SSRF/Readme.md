# SSRF

## Definition
  SSRF is Server side request forgery attack where attacker tricks server-side application to make unintended request to internal or external servers on their behalf. It exploits trust relationships to perform unauthorized actions.

  ![image](https://github.com/th3-r3sistanc3/Notes/assets/71440632/780605e7-8703-4139-bae8-75a24e3316af)

## Impact

1. Sensitive information leak.
2. Perform arbitrary command execution.
3. An SSRF exploit that causes connections to external third-party systems might result in malicious onward attacks. These can appear to originate from the organization hosting the vulnerable application. --> Request comming from organization hosting vulnerable application.

## Example
  Suppose there is a online shopping web application which has a functionality to check whether particular item is in stock or not. In these functionality, web server makes request to internal DB server which is not exposed to internet for checking in stock of item. However, if these requests is vulnerable to SSRF attack it may allow attacker to modify request to access sensitive data from DB server. As DB server will look this request is coming from trusted web server, it will serve request resulting in attacker getting sensitive information.

![image](https://github.com/th3-r3sistanc3/Notes/assets/71440632/f4d65ca3-3c0a-4344-9975-bbb5c737ea9e)

![image](https://github.com/th3-r3sistanc3/Notes/assets/71440632/077ee120-bd6a-4eba-a40c-47a1deb4c951)


  
