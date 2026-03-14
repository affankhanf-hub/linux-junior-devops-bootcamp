Points

1.WHAT IS LEMP?

LEMP = Linux + Nginx + MySQL + PHP-FPM

2. Browser ──→ Nginx ──→ PHP-FPM ──→ MySQL ──→ Response
  ↑                                            ↓
  └──────────────────←────────────────────────┘

3.Step-by-Step Flow

Step	What Happens
1	Browser requests a page (e.g., index.php)
2	Nginx receives the request
3	Nginx sees it's a PHP file and passes it to PHP-FPM
4	PHP-FPM processes the PHP code
5	PHP-FPM queries MySQL if needed
6	MySQL returns data to PHP-FPM
7	PHP-FPM generates HTML and sends back to Nginx
8	Nginx sends HTML to browser


4. PART 4: LAMP vs LEMP

Difference 1: Web Server Model
Aspect	LAMP (Apache)	LEMP (Nginx)
Model	Process/thread-based	Event-driven
How it works	Creates new process/thread per connection	Single process handles many connections
Memory usage	Higher (each connection uses memory)	Lower (can handle 10k+ connections)
Best for	Dynamic content, .htaccess	Static files, high concurrency, reverse proxy

Simple explanation:

    Apache = Creates a new waiter for each customer

    Nginx = One super-efficient waiter handling many customers at once



5. Difference 2: PHP Execution
Aspect	LAMP (Apache)	LEMP (Nginx)
PHP handling	PHP module inside Apache (mod_php)	Separate PHP-FPM service
How it works	Apache runs PHP directly	Nginx passes PHP to PHP-FPM
Communication	Within same process	Via network (fastcgi)
Isolation	PHP runs as Apache user	PHP-FPM can run as different user

Simple explanation:

    LAMP = Chef works in the same kitchen as waiter

    LEMP = Chef has separate kitchen, waiter brings orders


6. Difference 3: Operational Tuning
Aspect	LAMP	LEMP
Configuration	.htaccess files per directory	Centralized nginx config
Performance tuning	Adjust MaxClients, KeepAlive	Adjust worker_processes, worker_connections
Static files	Served by Apache	Nginx serves them extremely fast
Reverse proxy	Possible but complex	Built-in, excellent performance

7. Difference 4: When to Choose Which
Choose LAMP (Apache) When	Choose LEMP (Nginx) When
Using .htaccess files	Need high concurrency (many users)
Apps require Apache-specific modules	Serving lots of static files
Team knows Apache well	Need reverse proxy/load balancer
Shared hosting environment	Modern PHP apps (Laravel, Symfony)
Easy directory-level config	Better performance with PHP-FPM


8. Nginx (Web Server)

    Event-driven, asynchronous architecture

    Handles thousands of connections with low memory

    Excellent for static files and reverse proxy

    Cannot process PHP directly – passes to PHP-FPM

9. PHP-FPM (FastCGI Process Manager)

    Separate service that processes PHP files

    Receives PHP requests from Nginx

    More efficient than mod_php for high traffic

    Can run with different user permissions


10. MySQL (Database) – Same as LAMP

    Stores data

    Same as in LAMP stack


Practical lab


STEP-BY-STEP LEMP DEPLOYMENT

Install LEMP Stack

sudo apt install nginx mysql-server php-fpm php-mysql -y

Check Nginx

sudo systemctl status nginx
curl http://localhost | head -10

Check MySQL

sudo systemctl status mysql
sudo mysql -u root -e "SHOW DATABASES;"

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day13$ sudo systemctl status mysql
sudo mysql -u root -e "SHOW DATABASES;"
● mysql.service - MySQL Community Server
     Loaded: loaded (/usr/lib/systemd/system/mysql.service; enabled; preset: en>
     Active: active (running) since Fri 2026-03-13 20:22:56 CET; 15min ago
   Main PID: 1342 (mysqld)
     Status: "Server is operational"
      Tasks: 37 (limit: 4599)
     Memory: 407.4M (peak: 493.8M)
        CPU: 10.510s
     CGroup: /system.slice/mysql.service
             └─1342 /usr/sbin/mysqld

Mar 13 20:22:27 affan-khan-VirtualBox systemd[1]: Starting mysql.service - MySQ>
Mar 13 20:22:56 affan-khan-VirtualBox systemd[1]: Started mysql.service - MySQL>
lines 1-13/13 (END)


Check PHP-FPM

# Find your PHP version
php -v | head -1

# Check PHP-FPM service (version may differ)
sudo systemctl status php8.3-fpm


 Check PHP-FPM service (version may differ)
sudo systemctl status php8.3-fpm
PHP 8.3.6 (cli) (built: Jan 27 2026 03:09:47) (NTS)
● php8.3-fpm.service - The PHP 8.3 FastCGI Process Manager
     Loaded: loaded (/usr/lib/systemd/system/php8.3-fpm.service; enabled; prese>
     Active: active (running) since Fri 2026-03-13 20:33:07 CET; 15min ago
       Docs: man:php-fpm8.3(8)
    Process: 5936 ExecStartPost=/usr/lib/php/php-fpm-socket-helper install /run>
   Main PID: 5933 (php-fpm8.3)
     Status: "Processes active: 0, idle: 2, Requests: 0, slow: 0, Traffic: 0req>
      Tasks: 3 (limit: 4599)
     Memory: 7.6M (peak: 9.1M swap: 172.0K swap peak: 172.0K)
        CPU: 214ms
     CGroup: /system.slice/php8.3-fpm.service
             ├─5933 "php-fpm: master process (/etc/php/8.3/fpm/php-fpm.conf)"
             ├─5934 "php-fpm: pool www"
             └─5935 "php-fpm: pool www"

Mar 13 20:33:06 affan-khan-VirtualBox systemd[1]: Starting php8.3-fpm.service ->
Mar 13 20:33:07 affan-khan-VirtualBox systemd[1]: Started php8.3-fpm.service -



Configure Nginx to Use PHP-FPM

sudo nano /var/www/html/info.php


Test PHP via Nginx

curl http://localhost/info.php | head -20

ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day13$ curl http://localhost/info.php | head -20
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    21  100    21    0     0   5907      0 --:--:-- --:--:-- --:--:--  7000

<?php
phpinfo();
?>


our LEMP Stack Status
Component	Status
Nginx	✅ Working
PHP-FPM	✅ Working
PHP processing	✅ Working

LEMP is successfully deployed! 🎉


Quiz:

Question 1: Expand LEMP

Your answer: "Linux Nginx Mqsql PHP"

✅ CORRECT! (small typo "Mqsql" but clearly means MySQL)
LEMP = Linux, Nginx, MySQL, PHP
Question 2: Nginx and PHP communicate via what component?

Your answer: "FPM"

✅ CORRECT! PHP-FPM (FastCGI Process Manager) is the component that handles PHP processing for Nginx.


Question 3: One operational difference vs LAMP

Your answer: "LEMP has only one server which caters all the requests"

✅ CORRECT in concept! More precisely:

    LAMP (Apache) = Creates new process/thread per connection

    LEMP (Nginx) = Event-driven, single process handles many connections

Your answer captures the core idea ✅
Question 4: Why tune worker/process settings carefully?

Your answer: (blank)

Correct answer: To optimize performance for your specific workload. Too few workers = slow response. Too many = waste memory. Must balance based on traffic and server resources.
Question 5: When choose LEMP?

Your answer: "When traffic is very much"

✅ CORRECT! LEMP (Nginx) handles high concurrency better with less memory.

Also choose LEMP when:

    Serving lots of static files

    Using modern PHP frameworks

    Need reverse proxy

    Better performance with PHP-FPM

Hypothese

SCENARIO 1

You installed LEMP and created info.php. When you visit http://localhost/info.php, you see a blank page (no output).

Your Hypothesis: _________________________________

What command would you run to check? _________________________________

How would you fix it? _________________________________


Your Hypothesis: "PHP not installed"

✅ PARTIALLY CORRECT – PHP might be installed but not working properly.

Your Check Command: sudo systemctl status PHP3.FPM

⚠️ Almost correct – but the service name is php8.3-fpm (lowercase, with hyphen).

Better:
bash

sudo systemctl status php8.3-fpm

Your Fix: sudo apt install PHP3.FPM

⚠️ Wrong package name – should be:
bash

sudo apt install php-fpm php-mysql


Correct Answer for Scenario 2

Hypothesis: Nginx is not configured to pass PHP files to PHP-FPM, or PHP-FPM is not running.

Check:
bash

sudo systemctl status php8.3-fpm
ls /etc/nginx/sites-enabled/default  # Check nginx config

Fix:
bash

sudo systemctl start php8.3-fpm
sudo systemctl enable php8.3-fpm
sudo systemctl restart nginx

Also ensure nginx config has:
nginx

location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
}



Interview

Interviewer: "Can you compare LAMP and LEMP stacks?"

Web server: LAMP follows process/thread model .It creates a separate process for each connection.LEMP uses asynchronus model where 1 server deals multiple cconnections

Second, PHP execution: In apache PHP is part of process. NGINX PHP runs as a seperate service PHP-FPM

Operational tuning: Apache allows directory-level configuration via .htaccess files, which is flexible but can impact performance. Nginx uses centralized configuration and is tuned by adjusting worker processes. Nginx is particularly efficient at serving static files.

I will use Apache when team uses this well. When the application needs .htaccess support.Lemp for high traffic sites, applications with static sites or for Laravel 
