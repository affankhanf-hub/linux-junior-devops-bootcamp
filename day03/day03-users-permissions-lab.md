1. Anatomy of access . Who are you ? What you ar doing?
2. Three triplets 1)Owner 2) Group  3)Other
3. Permissions Read,Write and execute
   Read: Can see and list files 
   Write: Can write and delete data/files
   Execute : Run the program.Caan CD into it
4. Octal maths R=7 W=2 X=1
5. 750 concept Owner = R+W+X 7 4+2+1  Group R+0+X 4+0+1 Other 0 due to least privilege environment for safety . If others even have R so chances of data breach is high 
6. Sudo adduser creates new user Sudo adduser Affan
   Sudo groupadd creates new team Sudo groupadd webops
   Sudo usermod -aG webops affan  adds Affan into group webops
7. Chown -R Affan:webops: This tells the system: "This folder belongs to the Affan and the group webops."
   Chmod -R 750: This tells the system: "Apply the 750 math to this folder."
8. Adding or removing permssions by letter way using + and - 
   Add Execute	chmod +x file	Everyone can now run the file.
   Add Write to Group	chmod g+w file	The team can now edit the file.
   Remove Read from Others chmod o-r file Blocks "Others" from seeing the file.
9. The "Recursive" Change (-R) It tells Linux: "Start at the folder, change its permissions, then go inside and change every file and sub-folder inside it automatically."
10. By adding user thee system becomes secure. The guest concept

Practical lab:
Adding user
sudo adduser appuser
[sudo] password for affan-khan: 
info: Adding user `appuser' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `appuser' (1001) ...
info: Adding new user `appuser' (1001) with group `appuser (1001)' ...
info: Creating home directory `/home/appuser' ...
info: Copying files from `/etc/skel' ...
New password: 
BAD PASSWORD: No password supplied
Retype new password: 
Sorry, passwords do not match.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
Sorry, passwords do not match.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
[1]+  Stopped                 sudo adduser appuser

grep "appuser" /etc/passwd
appuser:x:1001:1001::/home/appuser:/bin/bash

Create the webops Group:

sudo groupadd webops

grep "webops" /etc/group
webops:x:1013:

User addition to group

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ sudo usermod -aG webops appuser

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ groups appuser
appuser : appuser webops

Phase 2 change of owner and group

sudo chown -R appuser:webops /srv/webapp
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ ls -ld /srv/webapp
Verification

drwxr-xr-x 2 appuser webops 4096 Feb 24 00:46 /srv/webapp

CHMOD Change of permissions (Taking the same folder example)

Checking existing permissions

ls -ld /srv/webapp
drwxr-xr-x 2 appuser webops 4096 Feb 24 00:46 /srv/webapp

755

After change 

sudo chmod -R 750 /srv/webapp
ffan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ ls -ld /srv/webapp
drwxr-x--- 2 appuser webops 4096 Feb 24 00:46 /srv/webapp

750

Audit and Proof

ls -ld /srv/webapp
drwxr-x--- 2 appuser webops 4096 Feb 24 00:46 /srv/webapp

750 is confirmed

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ sudo -u appuser touch /srv/webapp/boss_test.txt
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ ls /srv/webapp
ls: cannot open directory '/srv/webapp': Permission denied

Super user was able to create file but cannot check or list files. This indicates it was a success

Cleaning 

udo rm /srv/webapp/boss_test.txt
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp$ ls -la /srv/webapp
ls: cannot open directory '/srv/webapp': Permission denied


Flags

ool	Flag	Purpose	Hypothesis
chmod	-R	Recursive	Changes the folder and every file inside at once.
ls	-l	Long	Shows the rwxr-x--- and owners (appuser:webops).
ls	-d	Directory	Looks at the folder itself, not the files inside.
sudo	-u	User	Acts as appuser to test if the 7 (Owner) power works.
usermod	-aG	Append Group	Adds a user to webops without deleting their old groups.

Hypothesis:

f I set the folder to 750 and assign it to a specific team, then the application stays safe because the '7' lets the app work, the '5' lets the team watch, and the '0' keeps everyone else out.

Others:
ls -l /srv/webapp
ls: cannot open directory '/srv/webapp': Permission denied


Hypothesis : positive

Owner:

sudo -u appuser touch /srv/webapp/success.txt

sudo -u appuser touch /srv/webapp/success.txt
[sudo] password for affan-khan: 
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day03$ 


Hypotheis positive

Group:

ls -ld /srv/webapp
drwxr-x--- 2 appuser webops 4096 Feb 25 00:50 /srv/webapp

Positive

