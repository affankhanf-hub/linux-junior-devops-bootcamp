## Theory Summary

1. Anatomy of access means two things. First, who are you? Are you the owner, are you in the group, or are you just some other user. Second, what are you trying to do? Are you trying to read, write, or execute something.

2. Three triplets in Linux permissions. The first triplet is for the Owner. The second triplet is for the Group. The third triplet is for Other users. Each triplet has three positions for Read, Write, and Execute.

3. Read permission means you can see and list files in a directory. Write permission means you can create new files, delete existing files, and modify data inside files. Execute permission means you can run a program file and you can CD into a directory.

4. Octal maths works like this. Read is worth 4. Write is worth 2. Execute is worth 1. You add these numbers together to get the permission value. For example 4+2+1 equals 7 which means full access.

5. 750 concept explained simply. The Owner gets Read, Write, and Execute which is 4+2+1 equals 7. The Group gets Read and Execute only which is 4+0+1 equals 5. The Other user gets nothing which is 0. This follows the principle of least privilege which means give people only what they need and nothing more. If other users even have Read permission, the chances of a data breach become very high.

6. User and group management commands. sudo adduser creates a new user on the system. sudo groupadd creates a new team or group. sudo usermod -aG groupname username adds an existing user into an existing group. The -aG flags mean append to group so you do not remove the user from their other groups.

7. Ownership and permission commands explained. sudo chown -R username:groupname /path tells the system "This folder belongs to this user and this group." The -R flag makes it recursive so it applies to all files inside. sudo chmod -R 750 /path tells the system "Apply the 750 math to this folder and everything inside it."

8. Adding or removing permissions using letters. chmod +x file adds execute permission for everyone. chmod g+w file adds write permission for just the group. chmod o-r file removes read permission from other users. This letter method is useful when you only want to change one specific permission without touching the others.

9. The recursive flag -R explained. When you put -R in a command, it tells Linux "Start at this folder, change its permissions or ownership, then go inside and change every single file and sub-folder inside it automatically." Without -R, you only change the top folder and nothing inside.

10. Why adding users makes the system secure. When each person has their own user account, you can track who did what. You can give access to some people and deny access to others. You can remove one person without affecting anyone else. This is like giving each guest their own key card instead of making copies of the master key.

## Commands Used

| Command | Purpose |
|---------|---------|
| sudo adduser cloudserve-app | Create the application user |
| sudo adduser aadil | Create support engineer user |
| sudo adduser tahir | Create support engineer user |
| sudo groupadd cloudserve-support | Create the support team group |
| sudo usermod -aG cloudserve-support aadil | Add aadil to the support group |
| sudo usermod -aG cloudserve-support tahir | Add tahir to the support group |
| groups aadil | Verify aadil is in the correct group |
| groups tahir | Verify tahir is in the correct group |
| sudo mkdir -p /srv/cloudserve-app | Create the application directory |
| sudo chown -R cloudserve-app:cloudserve-support /srv/cloudserve-app | Change owner and group of the directory |
| sudo chmod -R 750 /srv/cloudserve-app | Set permissions to 750 on the directory |
| ls -ld /srv/cloudserve-app | Verify directory ownership and permissions |
| sudo adduser --disabled-password --gecos "" otheruser | Create a test user not in any group |
| sudo -u cloudserve-app touch /srv/cloudserve-app/owner-test.txt | Test if owner can write |
| sudo -u cloudserve-app ls -la /srv/cloudserve-app/ | Test if owner can read |
| sudo -u cloudserve-app bash -c "cd /srv/cloudserve-app && pwd" | Test if owner can execute (cd into folder) |
| sudo -u aadil bash -c "cd /srv/cloudserve-app && pwd" | Test if group member can execute |
| sudo -u aadil touch /srv/cloudserve-app/aadil-test.txt | Test if group member can write |
| sudo chmod 750 /srv/cloudserve-app/ | Fix permissions to 750 |
| sudo -u otheruser ls -ld /srv/cloudserve-app/ | Test if other user can read metadata |
| sudo -u otheruser bash -c "cd /srv/cloudserve-app && pwd" | Test if other user can execute |
| sudo -u otheruser touch /srv/cloudserve-app/other-test.txt | Test if other user can write |

## Output / Evidence

### Phase 1: Create Application User

```bash
$ sudo adduser cloudserve-app
info: Adding user `cloudserve-app' ...
info: Adding new group `cloudserve-app' (1001) ...
info: Adding new user `cloudserve-app' (1001) with group `cloudserve-app (1001)' ...
info: Creating home directory `/home/cloudserve-app' ...
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for cloudserve-app
        Full Name []: Affan Khan
        Room Number []: 1
        Work Phone []: 123
Is the information correct? [Y/n] y
info: Adding new user `cloudserve-app' to group `users' ...
Phase 2: Create Support Team Users
bash
$ sudo adduser aadil
(Similar output - user created successfully)

$ sudo adduser tahir
(Similar output - user created successfully)
Phase 3: Create Group and Add Users
bash
$ sudo groupadd cloudserve-support

$ sudo usermod -aG cloudserve-support aadil
$ sudo usermod -aG cloudserve-support tahir

$ groups aadil
aadil : aadil users cloudserve-support

$ groups tahir
tahir : tahir users cloudserve-support
Phase 4: Create Directory and Initial Permissions
bash
$ sudo mkdir -p /srv/cloudserve-app
$ sudo chown -R cloudserve-app:cloudserve-support /srv/cloudserve-app
$ sudo chmod -R 750 /srv/cloudserve-app

$ ls -ld /srv/cloudserve-app
drwxr-xr-x 2 cloudserve-app cloudserve-support 4096 Apr 26 21:08 /srv/cloudserve-app
Note: The output shows drwxr-xr-x which is 755, not 750. I discovered this mistake and fixed it later.

Phase 5: Create Other User for Testing
bash
$ sudo adduser --disabled-password --gecos "" otheruser
info: Adding user `otheruser' ...
info: Adding new group `otheruser' (1005) ...
info: Adding new user `otheruser' (1005) with group `otheruser (1005)' ...
info: Creating home directory `/home/otheruser' ...
Phase 6: Test Owner Access
bash
$ sudo -u cloudserve-app touch /srv/cloudserve-app/owner-test.txt
(no output - this means the command succeeded)

$ sudo -u cloudserve-app ls -la /srv/cloudserve-app/
total 8
drwxr-xr-x 2 cloudserve-app cloudserve-support 4096 Apr 26 21:11 .
drwxr-xr-x 3 root           root               4096 Apr 26 21:08 ..
-rw-rw-r-- 1 cloudserve-app cloudserve-app        0 Apr 26 21:12 owner-test.txt

$ sudo -u cloudserve-app bash -c "cd /srv/cloudserve-app && pwd"
/srv/cloudserve-app
These three tests prove the owner can write, read, and execute.

Phase 7: Test Group Member Access
bash
$ sudo -u aadil bash -c "cd /srv/cloudserve-app && pwd"
/srv/cloudserve-app

$ sudo -u aadil touch /srv/cloudserve-app/aadil-test.txt
touch: cannot touch '/srv/cloudserve-app/aadil-test.txt': Permission denied
The group member aadil could cd into the directory which means execute works. But he could not create a file which means write is blocked. This is exactly what 5 (r-x) should do.

Phase 8: Fix Permissions to 750
bash
$ sudo chmod 750 /srv/cloudserve-app/
$ ls -ld /srv/cloudserve-app/
drwxr-x--- 2 cloudserve-app cloudserve-support 4096 Apr 26 21:11 /srv/cloudserve-app/
Now the output shows drwxr-x--- which is the correct 750 permission.

Phase 9: Test Other User Access After Fix
bash
$ sudo -u otheruser ls -ld /srv/cloudserve-app/
drwxr-x--- 2 cloudserve-app cloudserve-support 4096 Apr 26 21:11 /srv/cloudserve-app/

$ sudo -u otheruser bash -c "cd /srv/cloudserve-app && pwd"
bash: line 1: cd: /srv/cloudserve-app: Permission denied

$ sudo -u otheruser touch /srv/cloudserve-app/other-test.txt
touch: cannot touch '/srv/cloudserve-app/other-test.txt': Permission denied
The other user could see the directory with ls -ld because that only reads metadata. But the real tests show they cannot cd in and cannot write. This proves they have no effective access.

Validation Summary Table
User Class	User	Permission	Write Test	Read Test	Execute (cd) Test	Status
Owner	cloudserve-app	7 (rwx)	✅ Created file	✅ Listed directory	✅ cd worked	FULL ACCESS
Group Member	aadil	5 (r-x)	❌ Permission denied	Not directly tested	✅ cd worked	READ ONLY
Other User	otheruser	0 (---)	❌ Permission denied	⚠️ Metadata only	❌ cd failed	NO ACCESS
Why 750 Was Chosen
The number 750 follows the octal maths where R=4, W=2, X=1. So 7 means 4+2+1 which gives Read, Write and Execute. 5 means 4+0+1 which gives Read and Execute but no Write. 0 means no access at all. I chose 750 for the CloudServe application folder because of the principle of least privilege which means give people only the access they absolutely need and nothing more.

The owner which is the application user cloudserve-app gets 7 meaning they can do everything. They can read the configuration files, write logs to the system, execute scripts to run the application, delete old files when needed, create new files for caching, and CD into the directory. The application needs full control to function properly in production.

The group which is the support team with users Aadil and Tahir gets 5 meaning they can only read and execute but cannot write. They can read the log files to see what errors are happening, they can CD into the directory to investigate, and they can list the contents to see what files exist. But they cannot write anything which means they cannot delete log files, cannot modify configuration, cannot create new files, and cannot change anything in the application folder. This is important because the support team should be able to troubleshoot by viewing logs but should never accidentally break the running application by changing or deleting files.

The other users who are not in the cloudserve-support group get 0 meaning they have no access at all. They cannot read any files, cannot write anything, cannot CD into the directory, cannot even list what is inside. This is the safest option because people who have no business with the application should not even see that the folder exists. At CloudServe, this includes contractors, interns, and employees from other departments who have no reason to touch the production application.

So in summary 750 works perfectly for this real world scenario. The application runs with full power using 7, the support team investigates using 5 but cannot change anything, and everyone else stays out completely using 0.

Troubleshooting
Issue 1: After running chmod 750, the permissions showed drwxr-xr-x instead of drwxr-x---

bash
$ ls -ld /srv/cloudserve-app
drwxr-xr-x 2 cloudserve-app cloudserve-support 4096 Apr 26 21:08 /srv/cloudserve-app
Root cause: I may have typed the command incorrectly or the command did not execute properly the first time.

Diagnosis: I checked with ls -ld and saw r-xr-x at the end. The last three characters r-x meant other users still had read and execute access. This was a security problem.

Resolution: I ran sudo chmod 750 /srv/cloudserve-app/ again and verified with ls -ld. The output changed to drwxr-x--- which is correct.

bash
$ sudo chmod 750 /srv/cloudserve-app/
$ ls -ld /srv/cloudserve-app/
drwxr-x--- 2 cloudserve-app cloudserve-support 4096 Apr 26 21:11 /srv/cloudserve-app/
Lesson: Always verify your permissions immediately after setting them. If you see the wrong output, run the command again.

Issue 2: The other user could still see the directory with ls -ld even after fixing permissions to 750.

bash
$ sudo -u otheruser ls -ld /srv/cloudserve-app/
drwxr-x--- 2 cloudserve-app cloudserve-support 4096 Apr 26 21:11 /srv/cloudserve-app/
Root cause: This is a WSL quirk. In WSL, the ls -ld command can read directory metadata even when execute permission is missing. This does not happen on a real Linux server.

Diagnosis: I tested with cd and touch which are the real tests for execute and write access. Both of these commands failed with Permission denied.

bash
$ sudo -u otheruser bash -c "cd /srv/cloudserve-app && pwd"
bash: line 1: cd: /srv/cloudserve-app: Permission denied

$ sudo -u otheruser touch /srv/cloudserve-app/other-test.txt
touch: cannot touch '/srv/cloudserve-app/other-test.txt': Permission denied
Resolution: No fix is needed because this is just a WSL behavior. The important tests show the user cannot cd in and cannot write. This means they have no effective access.

Lesson: In WSL, do not rely only on ls -ld to test permissions. Always test with cd or touch commands to prove what a user can actually do.

Flags Reference
Tool	Flag	Full Meaning	What It Does
chmod	-R	Recursive	Changes every file and folder inside the target folder
ls	-l	Long format	Shows the full permission string like drwxr-x---
ls	-d	Directory	Shows information about the folder itself, not what is inside
sudo	-u	User	Runs a command as if you are another user
usermod	-aG	Append to Group	Adds a user to a group without removing them from other groups

Lessons Learned

The command sudo -u username is very useful for testing. It lets me act as another user without logging out of my WSL session. I used this to test as cloudserve-app, aadil, and otheruser.

The permission string drwxr-x--- confirms 750 is applied. The d means directory. The rwx means owner has read, write, execute. The r-x means group has read and execute but no write. The --- means others have nothing.

In WSL, ls -ld can show directory metadata even when execute permission is missing. This is different from a real Linux server. The cd test is more reliable for proving execute permission.

The group member aadil could cd into the directory which proves execute works. But he could not create a file which proves write is blocked. This is exactly the r-x permission we wanted.

The other user could not cd into the directory and could not create a file. This proves they have no effective access even though ls -ld showed the directory name.

Always verify permissions immediately after chmod. I missed that my first chmod command did not work correctly. If I had not checked with ls -ld, I would not have known other users still had access.

When something does not work as expected, test with different commands. ls -ld told me one thing but cd and touch told me the real story.

