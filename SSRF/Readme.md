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

## Defenses

1. Blacklisting
  Blacklist the malicious/sensitive URLs
2. Whitelisting
  Allow only certain trusted URLs

## Bypassing defenses.

- Use an alternative IP representation of 127.0.0.1 such as 2130706433, 017700000001 or 127.1
- Obfuscate blocked strings using URL encoding or case variation. —> URL encoding or double URL encoding
- Try using different redirect codes, as well as different protocols for the target URL. For example, switching from an http: to https:
- Check if application is only checking if certain URL string is present in  request.https://expected-host:fakepassword@evil-host, https://evil-host#expected-host,  https://expected-host.evil-host
- Check open redirection vulnerability in the site whose URLs are whitelisted

## Blind SSRF vulnerabilities
  Blind SSRF vulnerabilities occur if you can cause an application to issue a back-end HTTP request to a supplied URL, but the response from the back-end request is not returned in the application's front-end response.
  Sometimes leads to full remote code execution on the server or other back-end components.

  - You can blindly sweep the internal IP address space, sending payloads designed to detect well-known vulnerabilities. If those payloads also employ blind out-of-band techniques, then you might uncover a critical vulnerability on an unpatched internal server.

  `It is common when testing for SSRF vulnerabilities to observe a DNS look-up for the supplied Collaborator domain, but no subsequent HTTP request. This typically happens because the application attempted to make an HTTP request to the domain, which caused the initial DNS lookup, but the actual HTTP request was blocked by network-level filtering. It is relatively common for infrastructure to allow outbound DNS traffic, since this is needed for so many purposes, but block HTTP connections to unexpected destinations.`

- Check SSRF in 'Referer Header'
