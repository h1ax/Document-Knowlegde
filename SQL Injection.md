# SQL Injection
_h1ax
## What is SQL Injection ?
[SQL Injection](https://en.wikipedia.org/wiki/SQL_injection) is the operations that take advantage of the gap in using the SQL database system (typically MySQL, ...)
It has been widely used to deal with websites or databases that are not carefully in protecting and often appear in the list of the most common bug.
## Detail
_Here is a simple php code to make a query to mySQL:_
```sh
$conn = new mysqli("localhost", "thaicom_nguyenhai", "pass_vjp_pro123", "thaicom_panel"); //this to call myPHPadmin to work with databases
$sql = "SELECT * FROM $table WHERE username = '$username' AND password = '$password'"; //this is code to make query to databases and it is a string.
        $result = mysqli_query($conn, $sql);     
        if(mysqli_num_rows($result) > 0) //if databases send more than 0 result > you in
        {
            <script type="text/javascript">
                alert("Login Success!");
                window.location.href='index.php';
            </script>
        }
```
So, you see in the second line: 
```sh
"SELECT * FROM $table WHERE username = '$username' AND password = '$password'"
```
_This using variable from the previous code._

If we type that: 
```sh
$username = 'admin'
$password = 'pass_admin'
```
Then the query will be: 
```sh
SELECT * FROM $table WHERE username='admin' AND password='pass_admin'
```
_It will work good and return 'admin' user in result (if exist)._
## BUT
There are sometime that we forget the password and know that our website is _not good at security._
Then we will do **SQL Injection**.

Okay, comeback to the code, what the problem if we do like this:
```sh
$username = 'admin'";'
$password = null
```
Then the real query will like this:
```sh
"SELECT * FROM $table WHERE username = 'admin'";' AND password = "
```
What will it do to the database ?
- It will just get the string before `;`
- Then it will be: `"SELECT * FROM $table WHERE username = 'admin'";`
- Server will just select user have `username='admin'` and forget to check the password.

And after that, you can go to your server **without input or input wrong password** :).

Why?
_- It just check which user is admin and send back the result. And it won't check what is the password._

**This is a simple SQL Injection attack.**'

## Defense
There are many way to defense SQL Injection. But I just tell more about 1 type. Other then you can go and search it :).
**Filtering the query: (not good but can use):**
- You can remove SQL Operation from the input like: 
```
+ addition
- subtraction
% modulus
BETWEEN
EXISTS
IN
NOT IN
LIKE
GLOB
NOT
OR
IS NULL
IS
IS NOT
||
UNIQUE
| bitwise or
& bitwise and
...
```
When user send an Injection to your server:
```sh
$username = 'admin\'";';
$password = '123';
"SELECT * FROM $table WHERE username = '$username' AND password = '$password'"
```
It will remove the character `'`, `"` and `;` in you query then it will like this:
Before:
``"SELECT * FROM $table WHERE username = 'admin'";`` `//' AND password = '123'"`
After filter:
``"SELECT * FROM $table WHERE username = 'admin' AND password = '123'"``
And it will also check with the query after filter remove Injection Operation.


**Encrypt the input and database (not bad)**
You can encrypt it when you add it to databases :D
Like this:
```sh
$username = 'admin';
$password = 'pass_admin';
$username = md5($username); //21232f297a57a5a743894a0e4a801fc3
$password = md5($password); //b0aaddaf46490fb756cfe63767e6b463
"INSERT INTO $tabel (`username`, `password`) VALUES ('$username', '$password')"
```
So the real query is:
``"INSERT INTO $tabel (`username`, `password`) VALUES ('21232f297a57a5a743894a0e4a801fc3', 'b0aaddaf46490fb756cfe63767e6b463')"``

Then, when you make query to your databases, you have to make md5 encrypt your input to access databases.
This is what I say:
```sh
$username = 'admin';
$password = 'pass_admin';
$username = md5($username); //21232f297a57a5a743894a0e4a801fc3
$password = md5($password); //b0aaddaf46490fb756cfe63767e6b463
"SELECT * FROM $table WHERE username = '$username' AND password = '$password'"
```
Real query is:
``"SELECT * FROM $table WHERE username = '21232f297a57a5a743894a0e4a801fc3' AND password = 'b0aaddaf46490fb756cfe63767e6b463'"``

And when someone try to attack you with SQL Injection ?
```sh
$username = 'admin\'";';
$password = '123';
$username = md5($username); //a1b27aa6a945cdcd5096b6ac6cd2fe29
$password = md5($password); //202cb962ac59075b964b07152d234b70
"SELECT * FROM $table WHERE username = '$username' AND password = '$password'"
```
Real query is:
``"SELECT * FROM $table WHERE username = 'a1b27aa6a945cdcd5096b6ac6cd2fe29' AND password = '202cb962ac59075b964b07152d234b70'"``

:) Then the server will return with nothing. -> You win.

**Using firewall to block SQL Injection (The best)**
[What is WAP](https://www.ptsecurity.com/ww-en/analytics/knowledge-base/how-to-prevent-sql-injection-attacks/)

**Remember:** as in many other cases in security engineering, no single solution should be considered the sole line of defense. Instead, a combined approach involving multiple security controls and best practices can help create a more robust and resilient security posture.
