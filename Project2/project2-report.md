Task 1 Static IP

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day13$ ip -4 -bra
Option "-bra" is unknown, try "ip -help".
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day13$ ip -4 -br a
lo               UNKNOWN        127.0.0.1/8 
enp0s3           UP             10.0.2.200/24 10.0.2.15/24 
docker0          DOWN           172.17.0.1/16 
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day13$ ip route |grep default
default via 10.0.2.2 dev enp0s3 proto static metric 100 
default via 10.0.2.2 dev enp0s3 proto dhcp src 10.0.2.15 metric 100 
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day13$ resolvectl status |grep "Dns servers"


Alraedy configured in the chapter's exercise


Task 2: Apply UFW Policy

ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day13$ # Check if UFW is installed
which ufw

# If not installed:
sudo apt install ufw -y

# Now set up firewall
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
sudo ufw status verbose
/usr/sbin/ufw
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
ufw is already the newest version (0.36.2-6).
0 upgraded, 0 newly installed, 0 to remove and 42 not upgraded.
Default incoming policy changed to 'deny'
(be sure to update your rules accordingly)
Default outgoing policy changed to 'allow'
(be sure to update your rules accordingly)
Skipping adding existing rule
Skipping adding existing rule (v6)
Skipping adding existing rule
Skipping adding existing rule (v6)
Skipping adding existing rule
Skipping adding existing rule (v6)
Firewall is active and enabled on system startup
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
80/tcp (Apache)            ALLOW IN    Anywhere                  
22/tcp                     ALLOW IN    Anywhere                  
80/tcp                     ALLOW IN    Anywhere                  
443/tcp                    ALLOW IN    Anywhere                  
80/tcp (Apache (v6))       ALLOW IN    Anywhere (v6)             
22/tcp (v6)                ALLOW IN    Anywhere (v6)             
80/tcp (v6)                ALLOW IN    Anywhere (v6)             
443/tcp (v6)               ALLOW IN    Anywhere (v6)             



sudo ufw status verbose

ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day13$ sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
80/tcp (Apache)            ALLOW IN    Anywhere                  
22/tcp                     ALLOW IN    Anywhere                  
80/tcp                     ALLOW IN    Anywhere                  
443/tcp                    ALLOW IN    Anywhere                  
80/tcp (Apache (v6))       ALLOW IN    Anywhere (v6)             
22/tcp (v6)                ALLOW IN    Anywhere (v6)             
80/tcp (v6)                ALLOW IN    Anywhere (v6)             
443/tcp (v6)               ALLOW IN    Anywhere (v6)       

Task 3: Deploy Static Site in Docker

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/Project2$ nano index.html
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/Project2$ nano Dockerfile
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/Project2$ dockerbuild -t project2-site:1.0
dockerbuild: command not found
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/Project2$ docker build -t project2-site:1.0 .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  3.072kB
Step 1/3 : FROM nginx:alpine
 ---> d0c780774910
Step 2/3 : COPY index.html /usr/share/nginx/html/index.html
 ---> 633d8c51433c
Step 3/3 : EXPOSE 80
 ---> Running in 458bd1c0a0ff
 ---> Removed intermediate container 458bd1c0a0ff
 ---> b01174b9abc4
Successfully built b01174b9abc4


Successfully tagged project2-site:1.0
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/Project2$ docker run -d --name project2 -p 8080:80 project2-site:1.0
8e6562cbfc16086479f4baa8758ebc27a9b3af784eb131e8da7c2cfbaca96872
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/Project2$ curl http://localhost:8080
My Project 2 static site in Docker

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/Project2$ docker ps
docker logs project2
CONTAINER ID   IMAGE               COMMAND                  CREATED          STATUS          PORTS                                     NAMES
8e6562cbfc16   project2-site:1.0   "/docker-entrypoint.…"   48 seconds ago   Up 47 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   project2
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/03/14 03:23:32 [notice] 1#1: using the "epoll" event method
2026/03/14 03:23:32 [notice] 1#1: nginx/1.29.6
2026/03/14 03:23:32 [notice] 1#1: built by gcc 15.
