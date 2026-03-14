Points

1.What is LAMP?

LAMP = Linux + Apache + MySQL + PHP

2. Component	What It Does	   Analogy
Linux	      Operating system	   The building
Apache	      Web server	   Front desk/receptionist
MySQL/MariaDB	Database	   File cabinet with all data
PHP	      Application runtime	Worker who processes requests

3. Request flow

Browser → Apache → PHP → MySQL
                           ↓
Browser ← Apache ← PHP ← (data)

Step	What Happens	Who Does It
1	You type http://yoursite.com in browser	Browser
2	Request goes to Apache (web server)	Apache receives it
3	Apache sees it's a PHP file, sends to PHP	Apache → PHP
4	PHP needs data, asks MySQL	PHP → MySQL
5	MySQL gives data back to PHP	MySQL → PHP
6	PHP generates HTML page, sends to Apache	PHP → Apache
7	Apache sends HTML page to browser	Apache → Browser
8	You see the webpage	Browser displays it


4.What is PHP?

PHP = A programming language that runs on the server to create dynamic web pages.

It geneerates ddynamic HTML

5. What is Apache?

Apache is a web server – a program that sits on a server and waits for people to request webpages, then sends those pages to their browser.

6.Apache's Job
Task	What Apache Does
Listen for requests	Waits on port 80 (HTTP) and 443 (HTTPS)
Find files	Looks for requested HTML, images, CSS in /var/www/html/
Run PHP	If file is .php, passes it to PHP processor
Send response	Sends the final HTML back to browser
Log everything	Records every request in log files

7. MY SQL/MARIA DB
MySQL stores all website data – user accounts, posts, products, comments. Data persists even when server restarts.

8. Static vs Dynamic

    Without PHP = Static HTML files (same for everyone)

    With PHP = Dynamic pages (can show different content per user)

9.Database Security

Never use MySQL root user in applications. Create dedicated database users with limited permissions for security.

10. Common Failure Modes

    Apache running but wrong virtual host

    Database authentication failures

    PHP module missing

    Firewall blocking ports (80/443)


Practical lab :

LAB TASK 1: Update Package List

sudo apt update

What this does:
Checks internet for latest software versions. Always do this before installing anything.

Why: Without this, you might install old versions or get errors.

LAB TASK 2: Install LAMP Stack

sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql -y


What each package does:
Package	What It Is	Simple Explanation
apache2	Web server	The program that answers when someone visits your site
mysql-server	Database	Stores all your site's data (users, posts, etc.)
php	Programming language	The "brain" that processes logic and talks to database
libapache2-mod-php	Apache-PHP connector	Lets Apache understand and run PHP files
php-mysql	PHP-MySQL connector	Lets PHP talk to MySQL database

LAB TASK 3: Check Apache is Running

sudo systemctl status apache2

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day12$ sudo systemctl status apache2
○ apache2.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/apache2.service; disabled; preset:>
     Active: inactive (dead)
       Docs: https://httpd.apache.org/docs/2.4/
Dead 

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day12$ sudo systemctl status apache2
○ apache2.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/apache2.service; disabled; preset:>
     Active: inactive (dead)
       Docs: https://httpd.apache.org/docs/2.4/
lines 1-4/4 (END)
[1]+  Stopped                 sudo systemctl status apache2
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day12$ affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day12$ sudo systemctl status apache2
○ apache2.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/apache2.service; disabled; preset:> 
     Active: inactive (dead)
       Docs: https://httpd.apache.org/docs/2.4/
bash: affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day12$: No such file or directory
○: command not found
bash: syntax error near unexpected token `('
bash: syntax error near unexpected token `('

Docs:: command not found
emctl stop nginx
sudo systemctl disable nginx
Synchronizing state of nginx.service with SysV service script with /usr/lib/systemd/systemd-sysv-install.
Executing: /usr/lib/systemd/systemd-sysv-install disable nginx
Removed "/etc/systemd/system/multi-user.target.wants/nginx.service".
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day12$ sudo systemctl start apache2
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day12$ sudo systemctl status apache2
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/apache2.service; disabled; preset:>
     Active: active (running) since Fri 2026-03-13 02:00:11 CET; 45s ago
       Docs: https://httpd.apache.org/docs/2.4/
    Process: 17715 ExecStart=/usr/sbin/apachectl start (code=exited, status=0/S>
   Main PID: 17718 (apache2)
      Tasks: 6 (limit: 4599)
     Memory: 21.9M (peak: 22.2M)
        CPU: 151ms
     CGroup: /system.slice/apache2.service




LAB TASK 4: Check MySQL is Running

sudo systemctl status mysql

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day12$ sudo systemctl status mysql
● mysql.service - MySQL Community Server
     Loaded: loaded (/usr/lib/systemd/system/mysql.service; enabled; preset: en>
     Active: active (running) since Fri 2026-03-13 01:53:40 CET; 11min ago
   Main PID: 17018 (mysqld)
     Status: "Server is operational"
      Tasks: 37 (limit: 4599)
     Memory: 351.0M (peak: 377.8M)
        CPU: 7.032s
     CGroup: /system.slice/mysql.service
             └─17018 /usr/sbin/mysqld


affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day12$ # Quick MySQL test
sudo mysql -u root -e "SHOW DATABASES;"
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+


LAB TASK 5: Secure MySQL Installation

sudo mysql_secure_installation

uccess.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : Y
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : Y
Success.


Task 6 

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day12$ # Create the PHP test file using nano
sudo nano /var/www/html/info.php

style type="text/css">
body {background-color: #fff; color: #222; font-family: sans-serif;}
pre {margin: 0; font-family: monospace;}
a:link {color: #009; text-decoration: none; background-color: #fff;}
a:hover {text-decoration: underline;}
table {border-collapse: collapse; border: 0; width: 934px; box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.2);}
.center {text-align: center;}
.center table {margin: 1em auto; text-align: left;}
.center th {text-align: center !important;}
td, th {border: 1px solid #666; font-size: 75%; vertical-align: baseline; padding: 4px 5px;}
th {position: sticky; top: 0; background: inherit;}
h1 {font-size: 150%;}
h2 {font-size: 125%;}
h2 a:link, h2 a:visited{color: inherit; background: inherit;}
.p {text-align: left;}
.e {background-color: #ccf; width: 300px; font-weight: bold;}
.h {background-color: #99c; font-weight: bold;}
100 76696    0 76696    0     0  1487k      0 --:--:-- --:--:-- --:--:-- 1497k
d; max-width: 300px; overflow-x: auto; word-wrap: break-word;}
curl: Failed writing body


LAB TASK 7: Create Test Database and User

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day12$ sudo mysql -u root -p <<EOF
CREATE DATABASE testdb;
CREATE USER 'testuser'@'localhost' IDENTIFIED BY 'password123';
GRANT ALL PRIVILEGES ON testdb.* TO 'testuser'@'localhost';
FLUSH PRIVILEGES;
EOF
[sudo] password for affan-khan: 
Enter password: 


LAB TASK 8: Create PHP Script to Test Database Connection

sudo nano /var/www/html/dbtest.php

Code:

<?php
$servername = "localhost";
$username = "testuser";
$password = "password123";
$dbname = "testdb";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}
echo "Connected successfully to MySQL database!";
$conn->close();
?>


What Each Line Does
Line	What It Does
$servername = "localhost";	Database is on this same computer
$username = "testuser";	The user we created in Task 7
$password = "password123";	Password for that user
$dbname = "testdb";	The database we created in Task 7
$conn = new mysqli(...);	Attempt to connect to MySQL
if ($conn->connect_error)	If connection fails, show error and stop
echo "Connected...";	If no error, show success message
$conn->close();	Close the connection (cleanup)

curl http://localhost/dbtest.php

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day12$ curl http://localhost/dbtest.php
Connected successfully to MySQL database!


TASK 10: Check Firewall

If it shows "Status: active":

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp


TASK 11: Create a Simple Info Page

<html>
<body>
<h1>My LAMP Server</h1>

<h2>Server Information</h2>
<ul>
    <li>Apache: <?php echo apache_get_version(); ?></li>
    <li>PHP Version: <?php echo phpversion(); ?></li>
    <li>MySQL: 
    <?php
    $conn = @new mysqli("localhost", "testuser", "password123", "testdb");
    if (!$conn->connect_error) {
        echo "✅ Connected";
        $conn->close();
    } else {
        echo "❌ Not connected";
    }
    ?>
    </li>
</ul>

<h2>Current Server Time</h2>
<p><?php echo date('Y-m-d H:i:s'); ?></p>

<h2>Test Links</h2>
<ul>
    <li><a href="/info.php">PHP Info</a></li>
    <li><a href="/dbtest.php">Database Test</a></li>
</ul>

</body>
</html>

curl http://localhost/index.php

# See who visited
sudo tail -20 /var/log/apache2/access.log

# See errors
sudo tail -20 /var/log/apache2/error.log


affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day12$ # See who visited
sudo tail -20 /var/log/apache2/access.log

# See errors
sudo tail -20 /var/log/apache2/error.log
::1 - - [13/Mar/2026:02:02:50 +0100] "GET / HTTP/1.1" 200 10926 "-" "curl/8.5.0"
::1 - - [13/Mar/2026:02:26:44 +0100] "GET /info.php HTTP/1.1" 200 76889 "-" "curl/8.5.0"
::1 - - [13/Mar/2026:04:01:27 +0100] "GET /dbtest.php HTTP/1.1" 200 189 "-" "curl/8.5.0"
::1 - - [13/Mar/2026:04:05:29 +0100] "GET /index.php HTTP/1.1" 200 574 "-" "curl/8.5.0"
[Fri Mar 13 02:00:11.466270 2026] [mpm_prefork:notice] [pid 17718] AH00163: Apache/2.4.58 (Ubuntu) configured -- resuming normal operations
[Fri Mar 13 02:00:11.466364 2026] [core:notice] [pid 17718] AH00094: Command line: '/usr/sbin/apache2'



html>
<body>
<h1>My LAMP Server</h1>

<h2>Server Information</h2>
<ul>
    <li>Apache: Apache/2.4.58 (Ubuntu)</li>
    <li>PHP Version: 8.3.6</li>
    <li>MySQL: 
    ✅ Connected    </li>
</ul>

<h2>Current Server Time</h2>
<p>2026-03-13 03:07:43</p>

<h2>Test Links</h2>
<ul>
    <li><a href="/info.php">PHP Info</a></li>
    <li><a href="/dbtest.php">Database Te


Command flags


Command 1: apt
bash

sudo apt update
sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql -y

Flag	Full Form	What It Does
-y	--yes	Automatically answer "yes" to prompts

Without -y: Apt will ask "Do you want to continue? [Y/n]" and wait for your input.
With -y: Installs without asking.
Command 2: systemctl
bash

sudo systemctl status apache2
sudo systemctl start apache2
sudo systemctl enable apache2

Flag	Full Form	What It Does
(no flags)	–	Just the command + service name

status = Check if running
start = Start now
enable = Start automatically at boot
Command 3: mysql
bash

sudo mysql -u root -p -e "SHOW DATABASES;"

Flag	Full Form	What It Does
-u	user	Specify which MySQL user (usually root)
-p	password	Prompt for password
-e	execute	Run SQL command and exit

Without -p: Won't ask for password (if none set)
With -p: Prompts for password
Command 4: curl
bash

curl http://localhost/info.php | head -20

Flag	Full Form	What It Does
(none)	–	Just fetch the page

| head -20 (not a curl flag) – shows only first 20 lines of output
Command 5: ufw
bash

sudo ufw status
sudo ufw allow 80/tcp

Flag	Full Form	What It Does
(none)	–	Just the command

allow 80/tcp = Open port 80 for TCP traffic
Command 6: nano
bash

sudo nano /var/www/html/filename.php

Flag	Full Form	What It Does
(none)	–	Opens the file for editing

No flags needed – just the filename


Quiz and my responses

Your Answers – Marked
Question 1: Expand LAMP

Your answer: "Linux, apache, mysql, php"

✅ CORRECT! LAMP = Linux, Apache, MySQL, PHP
Question 2: Apache role in stack?

Your answer: "Apache is a server which takes orders or instruction from the web"

✅ CORRECT! Apache receives browser requests and passes them to PHP if needed.
Question 3: Why separate DB user from root?

Your answer: "For security and privacy"

✅ CORRECT! Apps should never use root. Limited users = less damage if hacked.
Question 4: PHP runs where in LAMP path?

Your answer: "After SQL database execution"

❌ INCORRECT – PHP runs BEFORE database.

Correct flow: Browser → Apache → PHP → MySQL → Response

PHP asks MySQL for data, MySQL returns it to PHP, then PHP generates HTML.
Question 5: One hardening step for DB.

Your answer: "sudo mysql_secure-installation"

✅ CORRECT! That command removes anonymous users, test databases, and sets root password.


Hypothese

SCENARIO 1: PHP File Downloads Instead of Displaying

Your Hypothesis: "PHP version missing"

✅ CORRECT! The PHP module is not installed or not enabled.

Your Check Command: ls-la

❌ Not quite – ls-la is wrong (missing space). Should be ls -la but that just lists files.

Better check:
bash

ls /etc/apache2/mods-enabled/ | grep php

or
bash

php -v

Your Fix: sudo apt install php

✅ CORRECT! This installs PHP. You may also need:
bash

sudo systemctl restart apache2

Your Score: 2/3 – Good!


SCENARIO 2: Database Connection Error

Your Hypothesis: "Wrong user name" ✅ CORRECT! (or wrong password)

What command to check:
bash

sudo mysql -e "SELECT user, host FROM mysql.user;"

This shows all MySQL users. Check if testuser exists.

How to fix:
bash

sudo mysql -e "CREATE USER 'testuser'@'localhost' IDENTIFIED BY 'password123';"
sudo mysql -e "GRANT ALL PRIVILEGES ON testdb.* TO 'testuser'@'localhost';"
sudo mysql -e "FLUSH PRIVILEGES;"

Or if user exists but password wrong:
bash

sudo mysql -e "ALTER USER 'testuser'@'localhost' IDENTIFIED BY 'password123';"



SCENARIO 3: Wrong Page Shows Up

Your Hypothesis: Your index.php file is not in the correct folder or Apache is pointing to wrong location.

What command to check:
bash

ls -la /var/www/html/

See if your index.php file exists there.

How to fix:
bash

# Move your file to correct location
sudo mv ~/your-file.php /var/www/html/index.php

# Or create it if missing
sudo nano /var/www/html/index.php



SCENARIO 4

Problem: You try to log into MySQL:
bash

sudo mysql -u root -p

Output:
text

ERROR 2002 (HY000): Can't connect to local MySQL server through socket

Your Hypothesis:

What command would you run to check?

How would you fix it?
PHP-SQL not installed 

ls-la 

sudo apt install php-sql
Your Answer – Marked
SCENARIO 4: Can't Connect to MySQL

Your Hypothesis: "PHP-SQL not installed"

❌ INCORRECT – This error is about MySQL service not running, not PHP.

What the error means: MySQL server is stopped or not installed.

Your Check Command: ls-la

❌ Wrong – ls-la lists files. Need to check service status.

Correct check:
bash

sudo systemctl status mysql

Your Fix: sudo apt install php-sql

❌ Wrong fix – PHP MySQL extension is already installed. Need to start MySQL.

Correct fix:
bash

sudo systemctl start mysql
sudo systemctl enable mysql

Correct Answer for Scenario 4

Your Hypothesis: MySQL service is not running

Check command: sudo systemctl status mysql

Fix: sudo systemctl start mysql


SCENARIO 5

Problem: You can access your site from your own computer:
bash

curl http://localhost

Works fine.

But your friend on the same network tries http://192.168.1.100 and gets no response (connection timed out).

Your Hypothesis:

What command would you run to check?

How would you fix it?
no idea
No Problem! Here's the Answer
SCENARIO 5: Local Works, Remote Fails

Your Hypothesis: Firewall is blocking port 80 for external connections.

What command to check:
bash

sudo ufw status

If it shows "active", check if port 80 is allowed.

How to fix:
bash

sudo ufw allow 80/tcp
sudo ufw reload


