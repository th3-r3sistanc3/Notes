# Authentication

- Authentication is the process of verifying the identity of a user or client.

## Three main types of authentication

1. Something you know. --> passwords
2. Something you have. --> Keys, cards.
3. Something you are. --> biometrics

Authentication is the process of verifying that a user is who they claim to be. Authorization involves verifying whether a user is allowed to do something.

## ATTACKS

1. Brute-force attacks
2. Username enumeration
    - To check
        - Status codes
        - Error messages
        - Response time
3.

## Flawed Protection

1. IP based blocking the account that the remote user is trying to access if they make too many failed login attempts.
    - In some implementations, the counter for the number of failed attempts resets if the IP owner logs in successfully. This means an attacker would simply have to log in to their own account every few attempts to prevent this limit from ever being reached.

2. Account locking
    - Password Spraying
    - One Username 3 passs

## Flaws in Multi factor authentication

- It is only ever as secure as its implementation

1. If user is prompted to enter password and then prompted to enter verification on seperate page, chances are that user is already in logged in state. --> Try to navigate to logged in pages without entering OTP.

2. Sometimes flawed logic in two-factor authentication means that after a user has completed the initial login step, the website doesn't adequately verify that the same user is completing the second step.

3. Brute force 2FA code.

## Prevention

1. Take care with user credentials --> Never login over unencrypted channel.
2. Effective password policy
3. Prevent username enumeration --> Generic message.
4. Implement robust brute-force protection
5. Don't forget supplementary functionality --> Like 'remember me' and 'reset password' functionalities.
6. Implement proper multifactor authentication.

