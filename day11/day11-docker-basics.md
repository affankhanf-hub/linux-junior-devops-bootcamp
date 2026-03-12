Points summary

1.What is Docker?

Docker (platform) packages applications into containers – like a USB with software that runs anywhere.

2. Image vs Container (THE MOST IMPORTANT CONCEPT)
Concept  	What It Is	         USB Analogy
Image	     Read-only template 	The software on USB (not running)
Container	Running instance	The USB plugged in and running

3.Port Mapping

-p 8080:80
   ↑      ↑
 host   container
(your PC) (container)


4.Container Lifecycle

pull → run → ps/logs → stop → rm
  ↓      ↓       ↓       ↓      ↓
Download Start  Check   Stop   Delete

4.PORTS – Why We Need Them
The Problem

Containers run isolated. They have their own internal network. Your computer cannot see inside them by default.

5.Your Computer (host)          Container
─────────────────────────────────────────
Port 8080  ───────────────→  Port 80
(you browse localhost:8080)   (nginx listening inside)

6.Without Port Mapping

docker run -d nginx
# Container runs, but you CANNOT access it from browser
# It's isolated – no way in!

With

docker run -d -p 8080:80 nginx
# Now you can open browser to localhost:8080 and see nginx

7. VOLUMES – Keeping Data Safe

The Problem

Containers are temporary. When you delete a container, everything inside is gone forever.

docker run -d --name web1 nginx
# ... website creates data inside container
docker rm web1
# ALL DATA IS GONE! 💥

The Solution – Volumes

Volumes are like a USB drive plugged into the container. Data survives even if container is deleted

Practical lab

affan-khan@affan-khan-VirtualBox:~$ docker pull nginx:latest
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
docker.io/library/nginx:latest


affan-khan@affan-khan-VirtualBox:~$ docker run -d --name web1 -p 8080:80 nginx:latest
52ae130fe1f360b25ec41c12a560397e816b8fb078734516acf621aee9eaac

affan-khan@affan-khan-VirtualBox:~$ docker ps

CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                     NAMES
52ae130fe1f3   nginx:latest   "/docker-entrypoint.…"   23 seconds ago   Up 22 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/t

ffan-khan@affan-khan-VirtualBox:~$ curl http://localhost:8080
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
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy, 
API gateway, load balancer, content cache, or other features.<


Custom docker image

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ mkdir site
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ echo '<h1>My Custom Docker Site</h1>' > site/index.html
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ nano Dockerfile
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ ls -la
# Should show: Dockerfile  site/

ls -la site/
# Should show: index.html
total 16
drwxrwxr-x  3 affan-khan affan-khan 4096 Mar 12 03:05 .
drwxrwxr-x 15 affan-khan affan-khan 4096 Mar 12 02:47 ..
-rw-rw-r--  1 affan-khan docker       82 Mar 12 03:05 Dockerfile
drwxrwxr-x  2 affan-khan docker     4096 Mar 12 03:04 site
total 12
drwxrwxr-x 2 affan-khan docker     4096 Mar 12 03:04 .
drwxrwxr-x 3 affan-khan affan-khan 4096 Mar 12 03:05 ..

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ docker build -t my-site:1.0
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

docker: 'docker build' requires 1 argument

Usage:  docker build [OPTIONS] PATH | URL | -

Run 'docker build --help' for more information
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ docker run -d --name mysite -p 8081:80 my-site:1.0
Unable to find image 'my-site:1.0' locally
docker: Error response from daemon: pull access denied for my-site, repository does not exist or may require 'docker login': denied: requested access to the resource is denied

Run 'docker run --help' for more information
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ docker build -t my-site:1.0 .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  3.584kB
Step 1/3 : FROM nginx:alpine
alpine: Pulling from library/nginx
589002ba0eae: Pulling fs layer
d2a46166eee6: Pulling fs layer
593488f95c35: Pulling fs layer
e19aff8f2cce: Pulling fs layer
1549d7aec962: Pulling fs layer
1f25242adbdb: Pulling fs layer
c32126d2b96c: Pulling fs layer
c24026275c33: Pulling fs layer
e19aff8f2cce: Waiting
1549d7aec962: Waiting
1f25242adbdb: Waiting
c32126d2b96c: Waiting
c24026275c33: Waiting
593488f95c35: Verifying Checksum
593488f95c35: Download complete
e19aff8f2cce: Verifying Checksum
e19aff8f2cce: Download complete
1549d7aec962: Verifying Checksum
1549d7aec962: Download complete
d2a46166eee6: Verifying Checksum
d2a46166eee6: Download complete
1f25242adbdb: Verifying Checksum
1f25242adbdb: Download complete
c32126d2b96c: Verifying Checksum
c32126d2b96c: Download complete
589002ba0eae: Verifying Checksum
589002ba0eae: Download complete
589002ba0eae: Pull complete
d2a46166eee6: Pull complete
593488f95c35: Pull complete
e19aff8f2cce: Pull complete
1549d7aec962: Pull complete
1f25242adbdb: Pull complete
c32126d2b96c: Pull complete
c24026275c33: Verifying Checksum
c24026275c33: Download complete
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
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ docker run -d --name mysite -p 8081:80 my-site:1.0
d20e7d1e2a0e87a8a664f5018cce9e772d5118959841ec0c2b66ac7b30c737ed
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ curl http://localhost:8081
<h1>My Custom Docker Site</h1>
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ ^C
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ docker run -d --name mysite -p 8081:80 my-site:1.0
docker: Error response from daemon: Conflict. The container name "/mysite" is already in use by container "d20e7d1e2a0e87a8a664f5018cce9e772d5118959841ec0c2b66ac7b30c737ed". You have to remove (or rename) that container to be able to reuse that name.

Run 'docker run --help' for more information
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ ls
Dockerfile  site
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ rm -rf site
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ rm -f Dockerfile
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ ls -la
total 8
drwxrwxr-x  2 affan-khan affan-khan 4096 Mar 12 02:38 .
drwxrwxr-x 15 affan-khan affan-khan 4096 Mar 12 01:20 ..
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ mkdir site
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ cd site
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11/site$ nano site/index.html
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11/site$ cd ..
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11$ cd site
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11/site$ nano Dockerfile
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11/site$ ls -la
total 16
drwxrwxr-x 2 affan-khan docker     4096 Mar 12 02:41 .
drwxrwxr-x 3 affan-khan affan-khan 4096 Mar 12 02:38 ..
-rw-rw-r-- 1 affan-khan docker       82 Mar 12 02:41 Dockerfile
-rw-rw-r-- 1 affan-khan docker       73 Mar 12 02:39 site-index.html
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11/site$ ls -la site/
ls: cannot access 'site/': No such file or directory
affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootca

affan-khan@affan-khan-VirtualBox:~/linux-junior-devops-bootcamp/day11/site$ ls -la
total 16
drwxrwxr-x 2 affan-khan docker     4096 Mar 12 02:41 .
drwxrwxr-x 3 affan-khan affan-khan 4096 Mar 12 02:38 ..
-rw-rw-r-- 1 affan-khan docker       82 Mar 12 02:41 Dockerfile
-rw-rw-r-- 1 affan-khan docker       73 Mar 12 02:39 site-index.html


Command flags

docker run

docker run -d --name web1 -p 8080:80 nginx:latest

Flag	Full Form	What It Does
-d	detach	Run container in background
--name	name	Give container a custom name
-p	publish	Map port: host:container


docker build

docker build -t my-site:1.0 .

-t	tag	Give image a name:version

SCENARIO 1: Container Name Already Exists

Your Hypothesis: A container named web1 already exists (running or stopped).

Fix:

# Remove the existing container
docker rm web1

# OR use a different name
docker run -d --name web2 -p 8080:80 nginx:latest

SCENARIO 2: Build Fails - File Not Found

Your Hypothesis: The site/index.html file doesn't exist or path is wrong.

Fix:

# Check if file exists
ls -la site/

# Create it if missing
mkdir -p site
echo '<h1>My Site</h1>' > site/index.html

# Then rebuild
docker build -t my-site:1.0 .

SCENARIO 3: Wrong Port Mapping

Your Hypothesis: Container port should be 80 (nginx default), not 81.

Fix:

# Stop and remove wrong container
docker stop web1
docker rm web1

# Run with correct port mapping
docker run -d --name web1 -p 8080:80 nginx:latest

SCENARIO 4: Build Missing Context

Your Hypothesis: Missing the build context (the dot . at the end).

Fix:# Add the dot at the end
docker build -t my-site:1.0 .


SCENARIO 5: Container Runs But Can't Access

Your Hypothesis: Something wrong inside container – need to check logs.

Fix:

# Check logs to see what's happening
docker logs web1

# If nginx not starting properly, restart
docker stop web1
docker rm web1
docker run -d --name web1 -p 8080:80 nginx:latest




Quiz: My answers and feedback

Question 1: Difference between image and container?

Your answer: "Image is the software which you download. Container is the usb or the installation"

✅ CORRECT! Perfect analogy. Image = software on shelf. Container = software running.
Question 2: Command to list running containers?

Your answer: docker ps

✅ CORRECT! Exactly right.
Question 3: Purpose of Dockerfile?

Your answer: "To share the image and make it accessible"

⚠️ PARTIALLY CORRECT – Dockerfile is for building the image, not sharing it.

More accurate: Dockerfile is a recipe to build your own custom image. Sharing happens with docker push after building.
Question 4: Why map ports?

Your answer: "To avoid conflicts"

⚠️ PARTIALLY CORRECT – Port mapping is mainly to access the container from your computer.

More accurate: Map ports so you can reach the container (e.g., -p 8080:80 lets you visit at localhost:8080). Avoiding conflicts is a side benefit.
Question 5: Why use volume mounts?

Your answer: "To ensure data is preserved even if container is deleted"

✅ CORRECT! Exactly right. Volumes = persistent storage.



Interview

DAY 11 - Interview Questions
Docker Fundamentals
QUESTION 1

"Explain the difference between a Docker image and a container in simple terms."

Your answer:
QUESTION 2

"What command do you use to list running containers?"

Your answer:
QUESTION 3

"What is a Dockerfile and why do you need it?"

Your answer:
QUESTION 4

"Why do you need to map ports when running a container? What does -p 8080:80 mean?"

Your answer:
QUESTION 5

"What happens to data inside a container when you delete it? How do you prevent data loss?"

Your answer:
QUESTION 6

"Walk me through the steps to create your own Docker image with a custom website."

Your answer:
QUESTION 7

"What does docker pull nginx:latest do?"

Your answer:
QUESTION 8

"You run docker run -d --name web1 -p 8080:80 nginx and can't access it at localhost:8080. What do you check?"

Your answer:
QUESTION 9

"What's the difference between docker stop and docker rm?"

Your answer:
QUESTION 10

"Why would you build your own image instead of just using the official nginx image?"

Your answer:

Ready when you are. Pick any question and answer.


Your Answers – Marked
Question 1

Your answer: "Image is a software not running. Container is just like a usb plugged and program running"

✅ CORRECT! Perfect analogy. Image = software on shelf. Container = USB plugged in with program running.
Question 2

Your answer: docker ps

✅ CORRECT!
Question 3

Your answer: "To create an image"

✅ CORRECT! Dockerfile = recipe to build image.
Question 4

Your answer: "So that image reaches to the container. It means host port"

⚠️ Need to clarify – Port mapping is for YOU to reach the container, not for image to reach container.

Better: -p 8080:80 maps host port 8080 to container port 80 so you can access the container from your browser.
Question 5

Your answer: "It gets deleted. To preserve it we use volume"

✅ CORRECT! Data in container dies with it. Volumes save data.
Question 6

Your answer: "Html file - Dockerfile - Docker build - Docker run - Curl"

✅ CORRECT! Perfect sequence:

    HTML (your content)

    Dockerfile (instructions)

    docker build (create image)

    docker run (start container)

    curl (test it)

Question 7

Your answer: "It pulls latest version of nginx (Out of curriculum)"

✅ CORRECT! And yes, it's out of curriculum but good to know.
Question 8

Your answer: "Docker ps and check whether there is web1"

✅ CORRECT! Check if container exists and is running.
Question 9

Your answer: "Docker stop halts the program rm deletes it"

✅ CORRECT! Stop = pause, rm = delete permanently.
Question 10

Your answer: "To package our website with nginx"

✅ CORRECT! That's exactly why you build custom images.
Summary
Q	Status
1	✅ Correct
2	✅ Correct
3	✅ Correct
4	⚠️ Minor tweak
5	✅ Correct
6	✅ Correct
7	✅ Correct
8	✅ Correct
9	✅ Correct
10	✅ Correct

9.5/10 – Excellent!


