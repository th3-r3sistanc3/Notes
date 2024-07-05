# LFI

- Loading a file from a specified path.
- This is why we often see a parameter like /index.php?page=about, where index.php sets static content (e.g. header/footer), and then only pulls the dynamic content specified in the parameter, which in this case may be read from a file called about.php
- Vulnerable functions are
	- include_once(), require(), require_once(), file_get_contents()
	- readfile()
	- Response.WriteFile()
	- Many more
- Some of the above functions only read content while other may also execute, which can lead to code execution

![functions](https://github.com/th3-r3sistanc3/Notes/assets/71440632/b6e36509-091e-46b0-90f1-c54085ab7313)

## Bypasses

- Use absolute path
  > /etc/passwd
- If file is under some directory, use path traversal technique.
  > ../../../etc/passwd
- If application is appending some string to input for example (lang_: $GET['language']), then use / before our payload. (Not always works)
  > /../../../etc/passwd
- If application is appending some extensions, then use null characters.
  > ../../../etc/passwd%00

## Second Order Attacks

- As we can see, LFI attacks can come in different shapes. Another common, and a little bit more advanced, LFI attack is a Second Order Attack. This occurs because many web application functionalities may be insecurely pulling files from the back-end server based on user-controlled parameters.
- Exploiting LFI vulnerabilities using second-order attacks is similar to what we have discussed in this section. The only variance is that we need to spot a function that pulls a file based on a value we indirectly control and then try to control that value to exploit the vulnerability.
- 

