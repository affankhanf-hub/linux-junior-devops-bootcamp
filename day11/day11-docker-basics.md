Theory Summary

What is Docker?

Docker is a platform that packages applications into containers – like a USB with software that runs anywhere.

Image vs Container (THE MOST IMPORTANT CONCEPT)

| Concept | What It Is | USB Analogy |
|---------|-----------|--------------|
| Image | Read-only template | The software on USB (not running) |
| Container | Running instance | The USB plugged in and running |

Port Mapping
-p 8080:80
↑ ↑
host container
(your PC) (container)

text

Container Lifecycle
pull → run → ps/logs → stop → rm
↓ ↓ ↓ ↓ ↓
Download Start Check Stop Delete

text

Why Port Mapping is Needed

Containers run isolated. They have their own internal network. Your computer cannot see inside them by default.
Your Computer (host) Container
─────────────────────────────────────────
Port 8080 ───────────────→ Port 80
(you browse localhost:8080) (nginx listening inside)

text

**Without Port Mapping:**
```bash
docker run -d nginx
# Container runs, but you CANNOT access it from browser
With Port Mapping:

bash
docker run -d -p 8080:80 nginx
# Now you can open browser to localhost:8080 and see nginx
Volumes – Keeping Data Safe
The Problem: Containers are temporary. When you delete a container, everything inside is gone forever.

bash
docker run -d --name web1 nginx
docker rm web1
# ALL DATA IS GONE
The Solution – Volumes: Volumes are like a USB drive plugged into the container. Data survives even if container is deleted.

Commands Used

Command	Purpose
docker pull nginx:latest	Download nginx image from Docker Hub
docker run -d --name web1 -p 8080:80 nginx:latest	Run nginx container with port mapping
docker ps	List running containers
curl http://localhost:8080	Test if web server is accessible
mkdir site	Create directory for website files
echo '<h1>My Custom Docker Site</h1>' > site/index.html	Create custom HTML file
nano Dockerfile	Create Dockerfile recipe
docker build -t my-site:1.0 .	Build custom image
docker run -d --name mysite -p 8081:80 my-site:1.0	Run custom container
curl http://localhost:8081	Test custom site
Command Flags Reference
docker run

Flag	Full Form	What It Does
-d	detach	Run container in background
--name	name	Give container a custom name
-p	publish	Map host port to container port
docker build

Flag	Full Form	What It Does
-t	tag	Give image a name and version
Output / Evidence
Part 1: Pull and Run Official Nginx
Pull nginx image:

bash
$ docker pull nginx:latest
latest: Pulling from library/nginx
206356c42440: Pull complete
75a1d70aee50: Pull complete
a9d395129dce: Pull complete
df9da45c1db2: Pull complete
18a071c04bd1: Pull complete
79697674b897: Pull complete
9eef040df109: Pull complete
Digest: sha256:bc45d248c4e1d1709321de61566eb2b64d4f0e32765239d66573666be7f13349
Status: Downloaded newer image for nginx:latest
Run nginx container:

bash
$ docker run -d --name web1 -p 8080:80 nginx:latest
52ae130fe1f360b25ec41c12a560397e816b8fb078734516acf621aee9eaac
Verify container is running:

bash
$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                     NAMES
52ae130fe1f3   nginx:latest   "/docker-entrypoint.…"   23 seconds ago   Up 22 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   web1
Test nginx is accessible:

bash
$ curl http://localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>
</body>
</html>
Part 2: Build and Run Custom Image
Create website directory and file:

bash
$ mkdir site
$ echo '<h1>My Custom Docker Site</h1>' > site/index.html
$ ls -la site/
total 12
drwxrwxr-x 2 affan-khan affan-khan 4096 Mar 12 03:04 .
drwxrwxr-x 3 affan-khan affan-khan 4096 Mar 12 03:05 ..
-rw-rw-r-- 1 affan-khan affan-khan   82 Mar 12 03:04 index.html
Create Dockerfile:

bash
$ cat Dockerfile
FROM nginx:alpine
COPY site/index.html /usr/share/nginx/html/index.html
EXPOSE 80
Build custom image:

bash
$ docker build -t my-site:1.0 .
Sending build context to Docker daemon  3.584kB
Step 1/3 : FROM nginx:alpine
alpine: Pulling from library/nginx
589002ba0eae: Pull complete
d2a46166eee6: Pull complete
593488f95c35: Pull complete
e19aff8f2cce: Pull complete
1549d7aec962: Pull complete
1f25242adbdb: Pull complete
c32126d2b96c: Pull complete
c24026275c33: Pull complete
Digest: sha256:f46cb72c7df02710e693e863a983ac42f6a9579058a59a35f1ae36c9958e4ce0
Status: Downloaded newer image for nginx:alpine
 ---> d0c780774910
Step 2/3 : COPY site/index.html /usr/share/nginx/html/index.html
 ---> 48185e574955
Step 3/3 : EXPOSE 80
 ---> Running in 9723cc225150
 ---> Removed intermediate container 9723cc225150
 ---> 2417480714b8
Successfully built 2417480714b8
Successfully tagged my-site:1.0
Run custom container:

bash
$ docker run -d --name mysite -p 8081:80 my-site:1.0
d20e7d1e2a0e87a8a664f5018cce9e772d5118959841ec0c2b66ac7b30c737ed
Test custom site:

bash
$ curl http://localhost:8081
<h1>My Custom Docker Site</h1>
Validation
What I Tested	Command	Expected Result	My Result	Status
Pull nginx image	docker pull nginx:latest	Download complete	Download complete	✅ PASS
Run nginx container	docker run -d -p 8080:80 nginx	Container ID returned	52ae130fe1f3	✅ PASS
Container running	docker ps	Shows web1 container	web1 running	✅ PASS
Nginx accessible	curl localhost:8080	Welcome to nginx page	HTML returned	✅ PASS
Create custom HTML	ls site/	index.html exists	index.html present	✅ PASS
Build custom image	docker build -t my-site:1.0 .	Successfully built	2417480714b8	✅ PASS
Run custom container	docker run -d -p 8081:80 my-site:1.0	Container ID returned	d20e7d1e2a0e	✅ PASS
Custom site accessible	curl localhost:8081	My Custom Docker Site	HTML returned	✅ PASS

Troubleshooting
Failed Attempt 1: Missing Build Context
What I did wrong:

bash
$ docker build -t my-site:1.0
docker: 'docker build' requires 1 argument
Why it failed: I forgot the dot . at the end which specifies the build context (current directory).

How I fixed it:

bash
$ docker build -t my-site:1.0 .
Failed Attempt 2: Image Not Built Before Running
What I did wrong:

bash
$ docker run -d --name mysite -p 8081:80 my-site:1.0
Unable to find image 'my-site:1.0' locally
Why it failed: I tried to run the container before building the image.

How I fixed it: Built the image first with docker build -t my-site:1.0 ., then ran the container.

Failed Attempt 3: Container Name Conflict
What I did wrong:

bash
$ docker run -d --name mysite -p 8081:80 my-site:1.0
docker: Error response from daemon: Conflict. The container name "/mysite" is already in use.
Why it failed: A container named mysite already existed from previous run.

How I fixed it:

bash
$ docker rm mysite
$ docker run -d --name mysite -p 8081:80 my-site:1.0

Scenario Reference Guide
Scenario	Hypothesis	Fix

Container name already exists	A container with that name is already running or stopped	docker rm <name> or use a different name
Build fails - file not found	The site/index.html file doesn't exist or path is wrong	mkdir -p site and create index.html
Wrong port mapping	Container port should be 80 (nginx default)	Use -p 8080:80 not -p 8080:81
Build missing context	Missing the dot . at the end	Add . to docker build -t name:tag .
Container runs but can't access	Something wrong inside container	Check logs with docker logs <name>

Lessons Learned
Image vs Container: Image is the template (software on shelf). Container is the running instance (USB plugged in). This is the most important concept in Docker.

Port mapping is required because containers are isolated. -p 8080:80 maps host port 8080 to container port 80. Without this, you cannot access the container from your browser.

Dockerfile needs build context: Always include the dot . at the end of docker build -t name:tag . The dot tells Docker to use the current directory as build context.

Build before run: You cannot run a container from an image that hasn't been built yet. Sequence matters: build → run.

Container names must be unique: You cannot have two containers with the same name. Check docker ps -a to see existing containers. Remove old container with docker rm or use a different name.

Data in containers is temporary: When you delete a container, all data inside is lost forever. Use volumes (-v host:container) to persist data.

Dockerfile is a recipe: It tells Docker how to build your custom image step by step. FROM (base image), COPY (your files), EXPOSE (ports).

Always test with curl: After running a container, always test with curl to verify it is actually working, not just running.

Quiz Answers

Question	My Answer	Status
Difference between image and container?	Image is software not running. Container is like a USB plugged in with program running	✅ CORRECT
Command to list running containers?	docker ps	✅ CORRECT
Purpose of Dockerfile?	To create an image	✅ CORRECT
Why map ports? What does -p 8080:80 mean?	So you can access container from browser. Host port 8080 maps to container port 80	✅ CORRECT
Why use volume mounts?	To ensure data is preserved even if container is deleted	✅ CORRECT
