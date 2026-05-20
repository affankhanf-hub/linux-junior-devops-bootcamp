affanlinux@DESKTOP-INM25AC:~$ sudo adduser cloudserve-app
[sudo] password for affanlinux:
info: Adding user `cloudserve-app' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `cloudserve-app' (1001) ...
info: Adding new user `cloudserve-app' (1001) with group `cloudserve-app (1001)' ...
info: Creating home directory `/home/cloudserve-app' ...
info: Copying files from `/etc/skel' ...
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for cloudserve-app
Enter the new value, or press ENTER for the default
        Full Name []: Affan Khan
        Room Number []: 1
        Work Phone []: 123
        Home Phone []:
        Other []:
Is the information correct? [Y/n] y
info: Adding new user `cloudserve-app' to supplemental / extra groups `users' ...
info: Adding user `cloudserve-app' to group `users' ...

Create the other users (Aadil and Tahir)

affanlinux@DESKTOP-INM25AC:~$ sudo adduser aadil
[sudo] password for affanlinux:
info: Adding user `aadil' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `aadil' (1002) ...
info: Adding new user `aadil' (1002) with group `aadil (1002)' ...
info: Creating home directory `/home/aadil' ...
info: Copying files from `/etc/skel' ...
New password:
Retype new password:
Sorry, passwords do not match.
passwd: Authentication token manipulation error
passwd: password unchanged
Try again? [y/N] Y
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for aadil
Enter the new value, or press ENTER for the default
        Full Name []: Aadil
        Room Number []: 2
        Work Phone []: 345
        Home Phone []:
        Other []:
Is the information correct? [Y/n]
info: Adding new user `aadil' to supplemental / extra groups `users' ...
info: Adding user `aadil' to group `users' ...


affanlinux@DESKTOP-INM25AC:~$ sudo adduser tahir
info: Adding user `tahir' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `tahir' (1003) ...
info: Adding new user `tahir' (1003) with group `tahir (1003)' ...
info: Creating home directory `/home/tahir' ...
info: Copying files from `/etc/skel' ...
New password:
Retype new password:
Sorry, passwords do not match.
passwd: Authentication token manipulation error
passwd: password unchanged
Try again? [y/N] y
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for tahir
Enter the new value, or press ENTER for the default
        Full Name []: Tahir
        Room Number []: 3
        Work Phone []: 567
        Home Phone []:
        Other []:
Is the information correct? [Y/n] y
info: Adding new user `tahir' to supplemental / extra groups `users' ...
info: Adding user `tahir' to group `users' ...


Create the group

Add users to group

groups tahiraffanlinux@DESKTOP-INM25AC:~$
affanlinux@DESKTOP-INM25AC:~$ sudo usermod -aG cloudserve-support aadil
affanlinux@DESKTOP-INM25AC:~$ sudo usermod -aG cloudserve-support tahir
affanlinux@DESKTOP-INM25AC:~$
affanlinux@DESKTOP-INM25AC:~$ groups aadil
aadil : aadil users cloudserve-support
affanlinux@DESKTOP-INM25AC:~$ groups tahir
tahir : tahir users cloudserve-support
groups tahiraffanlinux@DESKTOP-INM25AC:~$
affanlinux@DESKTOP-INM25AC:~$ sudo usermod -aG cloudserve-support aadil
affanlinux@DESKTOP-INM25AC:~$ sudo usermod -aG cloudserve-support tahir
affanlinux@DESKTOP-INM25AC:~$
affanlinux@DESKTOP-INM25AC:~$ groups aadil
aadil : aadil users cloudserve-support
affanlinux@DESKTOP-INM25AC:~$ groups tahir
tahir : tahir users cloudserve-support


Create the test folder

affanlinux@DESKTOP-INM25AC:~$ sudo mkdir -p /srv/cloudserve-app
affanlinux@DESKTOP-INM25AC:~$ # Set ownership and permissions
hown -Raffanlinux@DESKTOP-INM25AC:~$ sudo chown -R cloudserve-app:cloudserve-support /srv/cloudserve-app
sudo chmod -R 750 /srv/cloudserve-appsudo chmod -R 750 /srv/cloudserve-appaffanlinux@DESKTOP-INM25AC:~$
affanlinux@DESKTOP-INM25AC:~$ Verify
ls -ld /srv/cloudserve-appVerify: command not found
affanlinux@DESKTOP-INM25AC:~$ ls -ld /srv/cloudserve-app
drwxr-xr-x 2 cloudserve-app cloudserve-support 4096 Apr 26 21:08 /srv/cloudserve-app

Create a test user for "Other User" testing
affanlinux@DESKTOP-INM25AC:~$ # Create a user NOT in the group
do addusaffanlinux@DESKTOP-INM25AC:~$ sudo adduser --disabled-password --gecos "" otheruser
info: Adding user `otheruser' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `otheruser' (1005) ...
info: Adding new user `otheruser' (1005) with group `otheruser (1005)' ...
info: Creating home directory `/home/otheruser' ...
info: Copying files from `/etc/skel' ...
info: Adding new user `otheruser' to supplemental / extra groups `users' ...
info: Adding user `otheruser' to group `users' ...

Now Test Each User Class Separately

TEST 1: OWNER (cloudserve-app) - Should have FULL access
sudo -u cloudserve-app bash -c "cd /srv/cloudserve-app && pwd"affanlinux@DESKTOP-INM25AC:~$
affanlinux@DESKTOP-INM25AC:~$ sudo -u cloudserve-app touch /srv/cloudserve-app/owner-test.txt
echo "Owner write test - Exit code: $?"
echo "Owner write test - Exit code: $?"
affanlinux@DESKTOP-INM25AC:~$
affanlinux@DESKTOP-INM25AC:~$ sudo -u cloudserve-app ls -la /srv/cloudserve-app/
total 8
drwxr-xr-x 2 cloudserve-app cloudserve-support 4096 Apr 26 21:11 .
drwxr-xr-x 3 root           root               4096 Apr 26 21:08 ..
-rw-rw-r-- 1 cloudserve-app cloudserve-app        0 Apr 26 21:12 owner-test.txt
affanlinux@DESKTOP-INM25AC:~$ sudo -u cloudserve-app bash -c "cd /srv/cloudserve-app && pwd"
/srv/cloudserve-app
a

TEST 2: GROUP MEMBER (aadil) - Should READ but NOT WRITE

TEST 2: GROUP MEMBER (aadil) - Should READ but NOT WRITE

affanlinux@DESKTOP-INM25AC:~$ sudo -u cloudserve-app ls -la /srv/cloudserve-app/
total 8
drwxr-xr-x 2 cloudserve-app cloudserve-support 4096 Apr 26 21:11 .
drwxr-xr-x 3 root           root               4096 Apr 26 21:08 ..
-rw-rw-r-- 1 cloudserve-app cloudserve-app        0 Apr 26 21:12 owner-test.txt


affanlinux@DESKTOP-INM25AC:~$ sudo -u aadil bash -c "cd /srv/cloudserve-app && pwd"
/srv/cloudserve-app

affanlinux@DESKTOP-INM25AC:~$ sudo -u aadil touch /srv/cloudserve-app/aadil-test.txt
cho "Group write test - Exit code: $?"echo "Group write test - Exit code: $?"touch: cannot touch '/srv/cloudserve-app/aadil-test.txt': Permission denied
affanlinux@DESKTOP-INM25AC:~$ sudo chmod 750 /srv/cloudserve-app/
affanlinux@DESKTOP-INM25AC:~$ ls -ld /srv/cloudserve-app/
drwxr-x--- 2 cloudserve-app cloudserve-support 4096 Apr 26 21:11 /srv/cloudserve-app/
affanlinux@DESKTOP-INM25AC:~$ sudo -u otheruser ls -ld /srv/cloudserve-app/
drwxr-x--- 2 cloudserve-app cloudserve-support 4096 Apr 26 21:11 /srv/cloudserve-app/
affanlinux@DESKTOP-INM25AC:~$ sudo -u otheruser bash -c "cd /srv/cloudserve-app && pwd"
bash: line 1: cd: /srv/cloudserve-app: Permission denied
affanlinux@DESKTOP-INM25AC:~$ sudo -u otheruser touch /srv/cloudserve-app/other-test.txt
touch: cannot touch '/srv/cloudserve-app/other-test.txt': Permission denied


