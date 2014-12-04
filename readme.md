Docker with boot2docker for Windows Tutorial
============================================

First follow Docker Installation for boot2docker
<https://docs.docker.com/installation/windows/>

![Alt text](/screenshots/installer.png?raw=true "Installer")

Note: I already had VirtualBox on my Windows 7 machine

Mac: The docker and boot2docker binaries are in /usr/local/bin which you can access from your terminal. 
Windows: C:\Program Files\Boot2Docker for Windows

Tip 1: where does the boot2docker VM (ISO) land on a Windows?
Windows: C:\Users\Burr\.boot2docker\boot2docker.iso
Mac: ~/.boot2docker/boot2docker.iso

Tip 2: where does the boot2docker instance land on Windows installation of VirtualBox
D:\Users\Burr\.VirtualBox\Machines\boot2docker-vm\Snapshots
Note: I have virtual box using a different drive on my machine

Tip 3: Window Size Width 160 
(http://screencast.com/t/Hjg7e0tSZs)
docker ps is best displayed with lots of width

1) boot2docker version
(optional) boot2docker upgrade (if on older version)
(http://screencast.com/t/Rhs3YsMvkF) - note: VirtualBox (installed prior to boot2docker has no mention of boot2docker)
2) boot2docker init
http://screencast.com/t/3Z38m5jKqrl
http://screencast.com/t/IUaW2kB0u
3) boot2docker up

watch for the follwing on Mac
To connect the Docker client to the Docker daemon, please set:
export DOCKER_TLS_VERIFY=1
export DOCKER_HOST=tcp://192.168.59.103:2376
export DOCKER_CERT_PATH=/Users/burr/.boot2docker/certs/boot2docker-vm

http://screencast.com/t/90gD6mN5

copy & paste those to execute, if you fail 
http://screencast.com/t/xnxbLxl50gsD


on Windows it says:
Waiting for VM and Docker daemon to start...
.....................ooooooooooooooooooooooooooo
Started.
Writing C:\Users\Burr\.boot2docker\certs\boot2docker-vm\ca.pem
Writing C:\Users\Burr\.boot2docker\certs\boot2docker-vm\cert.pem
Writing C:\Users\Burr\.boot2docker\certs\boot2docker-vm\key.pem
Docker client does not run on Windows for now. Please use
    "boot2docker" ssh
to SSH into the VM instead.
http://screencast.com/t/iUOXw8YL4M6J
http://screencast.com/t/13PZwb3E

4) boot2docker status
(time to shutdown) boot2docker down


5) boot2docker ip (good to know for later)
http://screencast.com/t/KIOC7nsj

6) boot2docker ssh
http://screencast.com/t/jkQqe9CYU

7) docker version
8) docker info
http://screencast.com/t/9es6HQHe

9) docker (lists commands)
http://screencast.com/t/yuu79Kn7OiF
 
10) docker images (any errors may be result of missing export - http://screencast.com/t/xnxbLxl50gsD
11) docker ps -a 
http://screencast.com/t/WHQUC5E5aV0B

12) docker run centos /bin/echo "Hello World"
http://screencast.com/t/uk9eFegGgr
This will take some time if this is the first run of the "centos" image
If you run the same command again, you will notice that is runs immediately, no download required.
A Docker container starts incredibly fast when compared to traditional virtual machine technology

13) On Windows, with boot2docker 1.3.x, the Users directory is shared as /c/Users
ls /c/Users
http://screencast.com/t/s1NXhix6eqHD

This shared folder will allow you to add and edit files using your traditional Windows tools
instead of having to learn vi or nano.

ls /c/Users/Burr/demo
http://screencast.com/t/fH7xOTLcoy9
http://screencast.com/t/gec9jWkt9CL


14) docker run -i -t centos /bin/bash
-i interactive 
-t allows your keyboard command 

cat /etc/system-release
http://screencast.com/t/DYgVMhGk

type "exit" to leave the container

15) docker ps
there are currently no running containers
http://screencast.com/t/0wGF9yUbReA

16) docker ps -a
there have been previously run containers
http://screencast.com/t/dexSD663nSBk

17) docker images
shows local images 
http://screencast.com/t/R0uA6jwR0Fl0

18) docker pull centos/wildfly
Docker Hub contains a large number of pre-configured images that are ready to use via a simple "pull"
https://registry.hub.docker.com/u/centos/wildfly/
Note: "run" does an implicit "pull" if the image is not already downloaded
Docker images are typically identified by two words "owner"/"imagename"
The centos/wildfly image includes nice documentation on how to use it - we will be following several of those steps
next.
http://screencast.com/t/NNLWkrvRd0

19) docker run -it centos/wildfly
http://screencast.com/t/rLq8QmHN
Note: "i" and "t" can be combined 
The "t" is important so you can "Ctrl-C" to stop wildfly and the container
Hit "Ctrl-C" and run a "docker ps" to see that the container has been stopped
In this case, the wildfly instance does not expose its port to the outside world, let's try that

20) docker run -it -p 8080:8080 centos/wildfly
Now, if you remember the IP address (from boot2docker ip) you can use your favorite browser to hit the server
http://screencast.com/t/rr1ibrZaY1
http://screencast.com/t/5AkNYFBjtq
and if you have forgotten your IP address, just open another Command Prompt and type "boot2docker ip"
http://screencast.com/t/5eslesheapd

Ctrl-C to terminate Wildfly and its container

21) docker history centos/wildfly
The history command allows you to see more detail into how the image was crafted
http://screencast.com/t/iZroijbqr3R

22) Now, let's modify this image by providing our own custom Java application, there will be several sub-steps
22a) If you remember way back to "ls /c/Users/Burr/demo", the "Users" directory on your Windows host
is shared with the boot2docker-vm (thanks to Virtual Box Guest Additions), create a directory called
"docker_projects" and then a sub-directory called "myapp"
http://screencast.com/t/dDcJeAP6N2gQ
and 
cd /c/Users/Burr/docker_projects/myapp

Note: "Burr" is the user name that this Windows session is logged in as, yours will be different.

22b)  In the "myapp" directory, create a text file called "Dockerfile", with no extension.  
http://screencast.com/t/m8RYW8yExw

22c) Edit the newly created Dockerfile, on Windows I tend to use Notepad++ 
add the following lines

FROM centos/wildfly
ADD html5java.war /opt/wildfly/standalone/deployments/

Note: the trailing "/" does matter

and you might be wondering where is "html5java.war", it is provided, just copy the .war into "myapp" directory

22d) Back in the boot2docker ssh session
docker build --tag=myapp .
http://screencast.com/t/OJyH5sI0

22e) Let's see if that worked
docker run -it -p 8080:8080 myapp
you should see the deployment in the wildfly console logging
http://screencast.com/t/UGoo88Or

And test the app via your browser
http://screencast.com/t/iKGGiKr4db

