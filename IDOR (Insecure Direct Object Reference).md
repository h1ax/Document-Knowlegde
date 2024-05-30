# IDOR (Insecure Direct Object Reference)
_h1ax_

Insecure Direct Object Reference (IDOR) is a security vulnerability that arises when a web application provides direct access to objects based on user-supplied input, without sufficient authentication and authorization checks. This flaw occurs when the application exposes internal implementation objects such as files, directories, database records, or keys. An attacker can exploit this vulnerability to gain unauthorized access to sensitive resources by manipulating the input that references these objects.

## I.Overview
1)How IDOR work ?
IDOR using the Unauthentication and Unauthorize in the web application. 
Example:
`read.php`
``` php
<?php
$data[0] = "h1ax";
$data[1] = "p00dle";
$data[2] = "abcdef";
$indexx = $_GET['id'];
echo $data[$indexx];
?>
```
When user using a GET request with query:
```
https://example.com/read.php?id=0
```
- `https://example.com/read.php` is the original link of the server
- `id=0` is the GET query side of the request
- Example: This is use to read data of user with id = `0`

Output:
```
h1ax
```
This is a simple code of showing the text. **But what will happen if we change the input query ?**
    - Like this:
```
https://example.com/read.php?id=1
```
**Server will be response the data in the index `1`:**
```
p00dle
```
-- This is an example of IDOR. It not only show the text like example. *Try to change to some private data like **Bank account, Password** -> It will be very dangerous.*

2)Impact of IDOR:
IDOR (Insecure Direct Object References) can have a substantial effect, such as:
- Disclosure of sensitive information: Without appropriate authentication, attackers can directly access and retrieve sensitive information from objects.
- Data manipulation is a significant issue for the system, as attackers have the ability to unlawfully modify the data of other objects.
- Data deletion: Without the necessary authorization, attackers have the ability to delete or destroy data from other objects.
- Disclosure of critical information: IDOR may result in the disclosure of critical information, including personal information and bank account details.

## II. Detecting IDOR
-- Try using skill and time to check each a little chance of bug.
-- Using tool like: Burpsuit, OWASP ZAP, ... to scan the bugs.
-- Review Source Code (If Accessible).
-- Check Logs and Error Reports.

## III. Preventing IDOR
1)Verification and Authorization Measures:
- Checking access permissions before granting access to objects.
- Proper authentication and authorization practices.

2)Using Encryption and Indirect References
- How to use encryption to avoid IDOR.
- Implementing indirect references instead of direct ones. 

3)Conducting Regular Security Audits
- Procedures for periodic security audits of applications.
- Tools and services that support security auditing.

## IV. Conclusion
In conclusion, IDOR (Insecure Direct Object References) is a major security issue that happens when programs provide direct access to objects based on user-supplied data without sufficient permission checks. This may lead to unauthorized access to sensitive data and significant data breaches.

To avoid IDOR threats, developers should implement rigorous access rules, utilize indirect references for important objects, and undertake extensive security testing. Continuous security education and incorporating vulnerability detection tools into the development process are also vital.

Understanding and avoiding IDOR not only increases application security but also protects an organization's reputation and assets from prospective attacks. Investing in security is vital, particularly given the rising complexity of cyberattacks.