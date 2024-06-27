# Path Traversal

- Path traversal is also known as directory traversal. These vulnerabilities enable an attacker to read arbitrary files on the server that is running an application.
- In some cases, an attacker might be able to write to arbitrary files on the server, allowing them to modify application data or behavior, and ultimately take full control of the server.
- The sequence `../` is valid within a file path, and means to step up one level in the directory structure.
- Windows --> `..\..\..\windows\win.ini`

## Bypasses

- You might be able to use an absolute path from the filesystem root, such as filename=/etc/passwd, to directly reference a file without using any traversal sequences.
- You might be able to use nested traversal sequences, such as ....// or ....\/
- Use `%2e%2e%2f` or `%252e%252e%252f`
- Use `..%c0%af` or `..%ef%bc%8f`
- An application may require the user-supplied filename to end with an expected file extension, such as .png. In this case, it might be possible to use a null byte to effectively terminate the file path before the required extension. For example: filename=../../../etc/passwd%00.

## Prevention

- The most effective way to prevent path traversal vulnerabilities is to avoid passing user-supplied input to filesystem APIs altogether
- compare the user input with a whitelist of permitted values
