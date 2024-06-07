# Server-side request forgery (SSRF)
_h1ax_

Server-Side Request Forgery (SSRF) is a security flaw that enables an unauthorized individual to manipulate the server into executing HTTP requests to any desired addresses provided by the attacker. Server-Side Request Forgery (SSRF) often happens when a web application retrieves information from an external origin using user input, without enough validation.

## I.Overview
1)How SSRF work ?
Application Vulnerability: The online application has a functionality that allows users to input a URL for data retrieval. For instance, a functionality that enables users to enter the URL of an image in order to exhibit it inside the program.

Lack of effective user input validation: The program fails to adequately verify the URL provided by the user. This enables the attacker to enter any URLs, including internal addresses or other services that the server may access.

Server Initiates the Request: Upon receiving a URL from the user, the server proceeds to execute an HTTP request to the specified URL in order to get the data. With SSRF, the attacker has the ability to provide any desired address in the URL, even internal addresses inside the server's network.

Server Response: The server processes the request and sends the result back to the attacker. If the URL refers to internal services, such as http://localhost or http://127.0.0.1, the attacker might potentially get sensitive information or gain unauthorized access to internal services that are not accessible from outside sources.
Example:
**read.php:**
```sh
<?php
$link = $_GET['url'];
echo file_get_contents($link);
```
This code to read contents from a url.
***Make a request:***
```sh
https://example/read.php?url=https://google.com
```
*It will read contents from `https://google.com` and print to the user.*
***Make a SSRF Attack Request:***
```sh
https://example/read.php?url=http://localhost:8080/adminxxx
```
- If the server does not properly validate the URL, it will make a request to http://localhost:8080/adminxxx and potentially return the content of the internal admin page to the attacker.

2)Impact of IDOR:
IDOR (Insecure Direct Object References) can have a substantial effect, such as:
- Access to Internal Resources: The attacker can access internal services and resources that are typically only accessible from within the network.
- Disclosure of Sensitive Information: The attacker can obtain sensitive information such as configuration details, user data, and other internal data.
- Further Attacks: SSRF can be used to perform further attacks such as exploiting vulnerabilities in internal services, carrying out denial-of-service (DoS) attacks, or leveraging the server to attack other targets.

## II. Detecting SSRF
-- Try using skill and time to check each a little chance of bug.
-- Using tool like: Burpsuit, OWASP ZAP, ... to scan the bugs.
-- Review Source Code (If Accessible).
-- Check Logs and Error Reports.

## III. Preventing IDOR
- Understand Application Functionality (Identify parts that accept user-provided URLs, such as file uploads, URL previews, and import/export features).
- Manual Testing (Craft Malicious Input: Test with internal URLs like `http://localhost`, `http://127.0.0.1`, or `http://169.254.169.254`).
- Automated Tools (BurpSuit, OWASP)
- Security Code Review (Check for direct use of user input in HTTP requests without proper validation).

## IV. Conclusion
Detecting and mitigating SSRF vulnerabilities is critical for online security. Use a mix of manual testing, automated tools (like Burp Suite and OWASP ZAP), out-of-band detection, and log reviews. Ensure correct input validation and security setups to avoid SSRF attacks. Implementing these precautions helps safeguard your application from illegal access and abuse.