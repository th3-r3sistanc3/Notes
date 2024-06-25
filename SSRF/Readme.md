# SSRF

## Definition
  SSRF is Server side request forgery attack where attacker tricks server-side application to make unintended request to internal or external servers on their behalf.

  ![image](https://github.com/th3-r3sistanc3/Notes/assets/71440632/780605e7-8703-4139-bae8-75a24e3316af)

## Impact

1. Sensitive information leak.
2. Perform arbitrary command execution.
3. An SSRF exploit that causes connections to external third-party systems might result in malicious onward attacks. These can appear to originate from the organization hosting the vulnerable application. --> Request comming from organization hosting vulnerable application

