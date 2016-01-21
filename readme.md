Docker with Docker Toolbox for Windows Tutorial
===============================================

This tutorial walks you through the basics of using a Java app server (WildFly) via a Linux container, running on Windows with  Docker Toolbox.  We have been testing this tutorial on Windows 7 and 8.1, you will notice that the screenshots come from either of those versions as this document has been tested and maintained.

There are notes for people running on Macs as well.


First follow [installation steps](https://docs.docker.com/windows/step_one/) for Docker Toolbox:

![Alt text](/screenshots/installer.png?raw=true "Installer")

Install all the components.

> Mac: The docker binaries are in `/usr/local/bin` which you can access from your terminal.

> Windows: The docker binaries lands in `C:\Program Files\Docker Toolbox` for Windows

* * *
##### Tip 1: Where does the boot2docker VM ISO land on a Windows?
Windows: `C:\Users\<your_username>\.docker\machine\cache\boot2docker.iso`

Mac: `~/.docker/machine/cache/boot2docker.iso`
* * *

##### Tip 2: Where does the docker-machine default instance land on Windows installation of VirtualBox?

`C:\Users\<your_username>\.docker\machine\machines\default\default`

* * *
##### Tip 3: Window Size Width 160 - docker ps is best displayed with lots of width.

You can make this change on the  `Docker Quickstart Terminal` command window as well

![Alt text](/screenshots/cmd_properties.png?raw=true "cmd.exe Properties")


* * *

##### Tip 4: VirtualBox before any `docker-machyine` instance.
![Alt text](/screenshots/virtualbox_before_boot2docker.png?raw=true "VirtualBox Before")
> VirtualBox installed prior to Docker Toolbox has no mention of any docker-machine until we create any docker-machine host.
> Also, if you have previously installed Docker Toolbox, you can often use `docker-machine upgrade <docker host name> ` to simply update to the latest version.
* * *

#### Explore Docker

1. Look for and select the `Docker Quickstart Terminal` menu option in your Start Menu

    ![Alt text](/screenshots/docker_quickstart_terminal.png?raw=true "Start Menu")

    >
    > Or use start.sh to launch the command prompt (not the normal Windows command prompt)
    >
    > You should be able to double-click on start.sh in C:\Program Files\Docker Toolbox

    ![Alt text](/screenshots/docker_toolbox_start.png?raw=true "start.sh")


    > If you successfully launch start.sh, it will execute `up`, `status` and `ip`, therefore you can skip to step 8 below.
    > Do make note of the IP address that is printed out, you will need it later.

    ![Alt text](/screenshots/start_sh_running.png?raw=true "start.sh running")

2. You might also execute docker-machine commands from the Windows (DOS) Command Prompt aka "cmd.exe" and type

    `docker-machine version`

    ![Alt text](/screenshots/docker_machine_version.png?raw=true "Command Prompt boot2docker version")

3. `docker-machine.exe create -d virtualbox default`
    ![Alt text](/screenshots/docker_machine_create.png?raw=true "docker_machine_create")
    > You should see the docker host called default listed in VirtualBox Manager

    ![Alt text](/screenshots/virtualbox_after_create.png?raw=true "VirtualBox After")

4. `docker-machine status default`
    > Note: When it is time to shutdown, run `docker-machine stop default`

5. `docker-machine env default`
    ![Alt text](/screenshots/docker_machine_env.png?raw=true "docker-machine env default")

    > This will show all environments variables needed to connect to the docker-machine host called default


6. `docker-machine env --shell cmd default`

    ![Alt text](/screenshots/docker_machine_env_cmd.png?raw=true "docker-machine env --shell cmd default")

    > Alternatively you can specify which type of shell you are using. So the proper instructions for environments variables will be created.

7. `docker-machine ssh default`
    ![Alt text](/screenshots/docker_machine_ssh.png?raw=true "docker-machine ssh default")

    > From this point forward, you will be inside of a Linux shell, using Linux commands

8. `docker version`


9. `docker info`
    ![Alt text](/screenshots/docker_info.png?raw=true "docker info")

10. `docker`
    ![Alt text](/screenshots/docker_lists_commands.png?raw=true "docker info")

11. `docker images`

12. `docker ps -a`
    ![Alt text](/screenshots/docker_ps_a.png?raw=true "docker ps -a")

13. `docker run centos /bin/echo "Hello World"`

    ![Alt text](/screenshots/docker_run_centos.png?raw=true "docker run centos")

    This will take some time if this is the first run of the "centos" image

    If you run the same command again, you will notice that is runs immediately, no download required.
    A Docker container starts incredibly fast when compared to traditional virtual machine technology.

    To prove that point, run the same command again.  

    > Note: the container stops as soon as it finishes the /bin/echo command

14. On Windows, with docker-machine, the Users directory is shared as `/c/Users`

    `ls /c/Users`

    ![Alt text](/screenshots/ls_users.png?raw=true "ls /c/Users")

    This shared folder will allow you to add and edit files using your traditional Windows tools instead of having to learn vi or nano.

    Using your File Explorer, create a `demo` sub-directory to your home directory and then use a `ls -l` to see it via the boot2docker-vm (in this example `Burr` is the username):

    `ls -l /c/Users/<your_username>/demo`

    ![Alt text](/screenshots/ls_l_c_users_Burr_demo.png?raw=true "ls -l /c/Users/Burr/demo")

    In this screenshot, I already some sample projects in my C:\Users\Burr\demo directory

    ![Alt text](/screenshots/windows_explorer.png?raw=true "Windows Explorer")

    Note: We won't be using "demo" in this tutorial, the goal here was to let you see the connection between /c/Users and C:\Users

15. `docker run -i -t centos /bin/bash`
    ````
    -i means interactive and
    -t allows your keyboard input
    ````

    You can also use `-it` as well as `-i -t`.  Remember this trick - if you have an app server failing to start,  you can see the console output and review the logs by using "-it"

    If this is your first time running the centos image, it may take over a minute to download.

    You are now running inside of the Centos-based container, to prove that point, use the following command

    `cat /etc/system-release`

    ![Alt text](/screenshots/cat_etc_system_release.png?raw=true "cat /etc/system-release")

    Type `exit` to leave the container and drop back into the boot2docker-vm shell.

16. `docker ps`

    There should be no currently running containers since `exit` terminated the last centos container
    ![Alt text](/screenshots/docker_ps.png?raw=true "docker ps")

17. `docker ps -a`

    but there have been previously run containers

    ![Alt text](/screenshots/docker_ps_a_2.png?raw=true "docker ps -a")

18. `docker images`

    shows local images
    ![Alt text](/screenshots/docker_images.png?raw=true "docker images")

19. `docker pull centos/wildfly`

    Docker Hub contains a large number of pre-configured images that are ready to use via a simple "pull" e.g. <https://registry.hub.docker.com/u/centos/wildfly/>

    > `run` does an implicit "pull" if the image is not already downloaded

    Docker images are typically identified by two words `"owner"/"imagename"`
    The `centos/wildfly` image includes nice documentation on how to use it - we will be following several of those steps next.

    ![Alt text](/screenshots/docker_pull_centos_wildfly.png?raw=true "docker pull centos/wildfly")

20. `docker run -it centos/wildfly`

    ![Alt text](/screenshots/docker_run_centos_wildfly.png?raw=true "docker run -it centos/wildfly")

    The `t` is important so you can `Ctrl-C` to stop wildfly and the container.

    Hit `Ctrl-C` and run a `docker ps` to see that the container has been stopped.

    In this particular case, the WildFly instance does not expose any ports to the outside world, let's try that next.

21. `docker run -it -p 8080:8080 centos/wildfly`

    ![Alt text](/screenshots/docker_run_it_p_8080_8080.png?raw=true "docker run -it -p 8080:8080 centos/wildfly")

    Each docker container process runs inside the docker host called `default` created by docker-machine previously.
    To get the ip of the docker host, just open another CMD.exe terminal and type `docker-machine ip default`.

    ![Alt text](/screenshots/browser_wildfly.png?raw=true "http://192.168.59.103:8080")


    Press `Ctrl-C` to terminate the WildFly container.

22. `docker history centos/wildfly`

    The history command allows you to see more detail into how the image was crafted
    ![Alt text](/screenshots/docker_history_centos_wildfly.png?raw=true "docker history centos/wildfly")

#### Modify the image and provide our own custom Java application

1. If you remember way back to `ls /c/Users/<your_username>/demo`, the `Users` directory on your Windows host
is shared with the docker `default` host (thanks to VirtualBox Guest Additions). In your home directory, create a directory called `docker_projects` that is a sibling of `demo`.
You can create the directory from within the docker-machine host `default` with the following command (or just use File Explorer).

    ````
    mkdir /c/Users/<your_username>/docker_projects
    ````
    > Use your home directory name in place of "Burr"

    and then create a sub-directory called `myapp`

    ````
    mkdir /c/Users/<your_username>/docker_projects/myapp
    ````

    > You can create the "myapp" directory via Windows Explorer or the boot2docker-vm shell

    ![Alt text](/screenshots/windows_explorer_myapp.png?raw=true "Windows Explorer myapp")

    and then change to the directory, it is important that you do this inside of the boot2docker-vm shell

    ````
    cd /c/Users/<your_username>/docker_projects/myapp
    ````

2. In the `myapp` directory, create a text file called `Dockerfile`, with no extension.

    > On Windows you might use the Atom editor from Atom.io for text editing.

    ![Alt text](/screenshots/dockerfile_windows_explorer.png?raw=true "Windows Explorer Dockerfile")


3.  Edit the newly created `Dockerfile` and add the following two lines:

    ````
    FROM centos/wildfly

    COPY javaee6angularjs.war /opt/wildfly/standalone/deployments/
    ````

    > Note: On Macs, we have seen Wildfly have a permissions problem with the .war.  The workaround is to switch to Root and use chown to make the ajustment to the .war file by adding the following two lines:

    ````
    USER root
    RUN chown wildfly:wildfly /opt/wildfly/standalone/deployments/javaee6angularjs.war
    ````

    > The trailing "/" does matter

    > You can find `javaee6angularjs.war` at <https://github.com/burrsutter/docker_tutorial/blob/master/javaee6angularjs.war?raw=true>
    > Download the war and copy it to the `myapp` directory.

4. Back in the boot2docker ssh session

    ````
    docker build --tag=myapp .
    ````

    > the trailing "." is important

    Use the docker `docker images` command to see if the image was created

    ![Alt text](/screenshots/after_docker_build.png?raw=true "after docker build")

5. Let's see if that worked

    ````
    docker run -it -p 8080:8080 myapp
    ````

    you should see the deployment of `javaee6angularjs.war` in the wildfly console logging

    ![Alt text](/screenshots/javaee6angularjs_myapp_startup.png?raw=true "docker run -it -p 8080:8080 myapp")



6. And test the app via your browser <http://192.168.99.100:8080/javaee6angularjs>

    > The IP address in my screenshots change from time to time as this document has been maintained. Just make sure to remember YOUR IP address as seen via `docker-machine ip default`

    ![Alt text](/screenshots/browser_javaee6angularjs_myapp.png?raw=true "http://192.168.99.100:8080/javaee6angularjs")

    > Now it is time for a victory dance around the room! You have your first Java EE application deployed as part of a Docker container.
    > Remember, Ctrl-C to shut down the app server.


#### Extra Credit

1. Run detached

    > You could also use a `-d` instead of `-it` to run the container detached, in the background

    ````
    docker run -d -p 8080:8080 myapp
    ````

    ![Alt text](/screenshots/docker_run_d.png?raw=true "docker run -d -p 8080:8080 myapp")


    > If detached, you will need to use `docker ps` to see the active containers and then use `docker stop CONTAINER_ID` and `docker rm CONTAINER_ID`
    >

    > Note: Docker automatically generated the name "agitated_hawking" which you can use instead of the CONTAINER_ID

    ![Alt text](/screenshots/docker_rm_agitated_hawking.png?raw=true "docker rm agitated_hawking")


2. Container naming

    > Adding a --name=some_name allows you to give override the default name of agitated_hawking or whatever was randomly assigned to your container by Docker

    ````
    docker run --name=myapp_is_running -d -p 8080:8080 myapp
    ````

    ![Alt text](/screenshots/myapp_is_running.png?raw=true "docker run --name=myapp_is_running -d -p 8080:8080 myapp")

3. Viewing logs

    ````
    docker logs myapp_is_running
    ````

    > and you can `docker stop myapp_is_running` when it is time to shutdown the -d detached app server container

    ![Alt text](/screenshots/docker_stop_myapp_is_running.png?raw=true "docker stop myapp_is_running")

4. Dive into a live container

     ````
     docker exec -it myapp_is_running bash

     cd /opt/wildfly/standalone/log
     tail server.log
     ````

     > This is a very useful technique if you find things are misbehaving and you wish poke around inside the running container.  

     ![Alt text](/screenshots/docker_exec.png?raw=true "docker exec -it myapp_is_running bash")

#### Cleanup

OPTIONAL - Clean Slate: If you wish to completely clean up and run through the above steps again:

1. Remove/Delete all containers

    ````
    docker rm `docker ps -a -q`
    ````

    >  the back ticks are important! You might also need to "stop" or "kill" any containers that are running and will not remove.


    ````
    docker ps -a
    docker stop CONTAINER_ID
    docker kill CONTAINER_ID
    ````

    Replace CONTAINER_ID with the id seen in the `docker ps` results.

2. Remove/Delete all images

    ````
    docker rmi `docker images -a -q`
    ````

    > watch those back ticks again

Check out the follow-on tutorial for adding MySQL.
<https://github.com/burrsutter/docker_mysql_tutorial>
