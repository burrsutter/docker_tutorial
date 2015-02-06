Docker with boot2docker for Windows Tutorial
============================================

This tutorial walks you through the basics of using a Java app server (WildFly) via a Linux container, running on Windows with boot2docker.  We have been testing this tutorial on Windows 7 and 8.1, you will notice that the screenshots come from either of those versions as this document has been tested and maintained.

There are notes for people running on Macs as well.

First follow [installation steps](https://docs.docker.com/installation/windows/) for boot2docker:

![Alt text](/screenshots/installer.png?raw=true "Installer")

Unless you already have VirtualBox installed, install all the components.

> Mac: The docker and boot2docker binaries are in `/usr/local/bin` which you can access from your terminal.
> Windows: The boot2docker binary lands in `C:\Program Files\Boot2Docker` for Windows

* * *
##### Tip 1: where does the boot2docker VM ISO land on a Windows?
Windows: `C:\Users\Burr\.boot2docker\boot2docker.iso`

Mac: `~/.boot2docker/boot2docker.iso`
* * *

##### Tip 2: where does the boot2docker instance land on Windows installation of VirtualBox

C:\Users\Burr\VirtualBox VMs\boot2docker-vm

* * *
##### Tip 3: Window Size Width 160 - docker ps is best displayed with lots of width

You can make this change on the Boot2Docker Start command window as well

![Alt text](/screenshots/cmd_properties.png?raw=true "cmd.exe Properties")


* * *

##### Tip 4: VirtualBox before `boot2docker init`
![Alt text](/screenshots/virtualbox_before_boot2docker.png?raw=true "VirtualBox Before")
> VirtualBox installed prior to boot2docker has no mention of boot2docker until we use `boot2docker init`.
> Also, if you have previously installed boot2docker, you can often use `boot2docker upgrade` to simply update to the latest version.
* * *

#### Explore Docker

1. Look for and select the `Boot2Docker Start` menu option in your Start Menu

    ![Alt text](/screenshots/boot2docker_start_menu.png?raw=true "Start Menu")

    >
    > Or use start.sh to launch the command prompt (not the normal Windows command prompt)
    >
    > You should be able to double-click on start.sh in C:\Program Files\Boot2Docker for Windows

    ![Alt text](/screenshots/boot2docker_start_sh.png?raw=true "start.sh")


    > If you successfully launch start.sh, it will execute `up`, `status` and `ip`, therefore you can skip to step 8 below.
    > Do make note of the IP address that is printed out, you will need it later.

    ![Alt text](/screenshots/start_sh_running.png?raw=true "start.sh running")

    > If there are error messages (and there normally are for a brand new installation), read steps 2 through 7 for some alternative ways to get boot2docker up and happy


2. You might also execute boot2docker commands from the Windows (DOS) Command Prompt aka "cmd.exe" and type

    `boot2docker version`

    ![Alt text](/screenshots/boot2docker_version.png?raw=true "Command Prompt boot2docker version")
    > Note: We have seen this fail on some systems.  There will be some workarounds listed in step 3 below.

    > Tip: on Windows 8.1, the Command Prompt is accessible if you hit the Windows key and X then C on the keyboard

3. `boot2docker init`
    ![Alt text](/screenshots/boot2docker_init.png?raw=true "boot2docker init")
    > You should see the boot2docker-vm listed in VirtualBox Manager

    ![Alt text](/screenshots/virtualbox_after_boot2docker.png?raw=true "VirtualBox After")

    > Note: This might result in

    ````
    error in run: Error generating new SSH Key into C:\Users\Burr\.ssh\id_boot2docker: exec: "ssh-keygen": executable file not found in %PATH%
    ````

    > If so, then fall back to the Boot2Docker Start menu option calling "start.sh".



4. `boot2docker up`

    Look for the following on _Windows_:

    ![Alt text](/screenshots/after_boot2docker_up.png?raw=true "boot2docker up")

    > Now, if you check the VirtualBox GUI you will see the `boot2docker-vm` running:

    ![Alt text](/screenshots/after_boot2docker_up_virtual_box.png?raw=true "boot2docker up results in VirtualBox")

    > Watch out for "VT-x is disabled in BIOS" errors
    > If virtualization has not been enabled in your machines BIOS, you could see the following error:

    ![Alt text](/screenshots/virtualization_error.png?raw=true "virtualization error")

    > Or, if you hit the Start button inside the VirtualBox Manager directly

    ![Alt text](/screenshots/VT-x_error_VirtualBox.png?raw=true "virtualization error in VirtualBox Manager")

    > If you see this error, you will need to update your BIOS settings accordingly.  

    > BIOS for Lenovo T440s

    ![Alt text](/screenshots/bios_lenovo_t440s.jpg?raw=true "Virtualization in Lenovo T440s BIOS")


    Watch for the following on _Mac OSX_:

    ![Alt text](/screenshots/macosx_env_vars.png?raw=true "Mac OSX Env Vars")

    Copy and paste the export statements you are provided with in to your terminal. If you get this step wrong, when you try further commands, you may see an error message like:

    ![Alt text](/screenshots/failed_macosx_env_vars.png?raw=true "Fail Mac OSX Env Vars")


5. `boot2docker status`
    > Note: When it is time to shutdown, run `boot2docker down`

6. `boot2docker ip`
    ![Alt text](/screenshots/boot2docker_ip.png?raw=true "boot2docker ip")

    > You will need this later!

7. `boot2docker ssh`
    ![Alt text](/screenshots/boot2docker_ssh.png?raw=true "boot2docker ssh")

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

14. On Windows, with boot2docker 1.3.x, the Users directory is shared as `/c/Users`

    `ls /c/Users`
    ![Alt text](/screenshots/ls_users.png?raw=true "ls /c/Users")

    This shared folder will allow you to add and edit files using your traditional Windows tools instead of having to learn vi or nano.

    Using your File Explorer, create a `demo` sub-directory to your home directory and then use a `ls -l` to see it via the boot2docker-vm (in this example `Burr` is the username):

    `ls -l /c/Users/Burr/demo`

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

    If you remember the IP address you can use your favorite browser to hit the server.
    If you forgot to make note of your IP address earlier, you can open another session into boot2docker.  Just go back to the Windows Start menu and select `Boot2Docker Start` or run start.sh.  You might wish to keep both boot2docker sessions open as it allows you to docker run an app server via "-it" in one window and then "docker ps" or "docker logs" in another window.

    ![Alt text](/screenshots/browser_wildfly.png?raw=true "http://192.168.59.103:8080")


    Press `Ctrl-C` to terminate the WildFly container.

22. `docker history centos/wildfly`

    The history command allows you to see more detail into how the image was crafted
    ![Alt text](/screenshots/docker_history_centos_wildfly.png?raw=true "docker history centos/wildfly")

#### Modify the image and provide our own custom Java application

1. If you remember way back to `ls /c/Users/Burr/demo`, the `Users` directory on your Windows host
is shared with the boot2docker-vm (thanks to VirtualBox Guest Additions). In your home directory, create a directory called `docker_projects` that is a sibling of `demo`.
You can create the directory from within the boot2docker-vm with the following command (or just use File Explorer).

    ````
    mkdir /c/Users/Burr/docker_projects
    ````
    > Use your home directory name in place of "Burr"

    and then create a sub-directory called `myapp`

    ````
    mkdir /c/Users/Burr/docker_projects/myapp
    ````

    > You can create the "myapp" directory via Windows Explorer or the boot2docker-vm shell

    ![Alt text](/screenshots/windows_explorer_myapp.png?raw=true "Windows Explorer myapp")

    and then change to the directory, it is important that you do this inside of the boot2docker-vm shell

    ````
    cd /c/Users/Burr/docker_projects/myapp
    ````

2. In the `myapp` directory, create a text file called `Dockerfile`, with no extension.

    > On Windows you might use the Atom editor from Atom.io for text editing.

    ![Alt text](/screenshots/dockerfile_windows_explorer.png?raw=true "Windows Explorer Dockerfile")


3.  Edit the newly created `Dockerfile` and add the following two lines:

    ````
    FROM centos/wildfly

    COPY javaee6angularjs.war /opt/wildfly/standalone/deployments/
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


    > Note: On Macs, we have seen Wildfly have a permissions problem with the .war.  The workaround is to switch to Root and use chown to make the ajustment to the .war file by adding the following two lines:

    ````
    USER root
    RUN chown wildfly:wildfly /opt/wildfly/standalone/deployments/javaee6angularjs.war
    ````


6. And test the app via your browser <http://192.168.59.105:8080/javaee6angularjs>

    > The IP address in my screenshots change from time to time as this document has been maintained. Just make sure to remember YOUR IP address as seen via start.sh or boot2docker ip

    ![Alt text](/screenshots/browser_javaee6angularjs_myapp.png?raw=true "http://192.168.59.105:8080/javaee6angularjs")

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

     cd /opt/wildfly/stanalone/log
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

3. Exit the boot2docker-vm shell, back at the Windows Command Prompt

    ````
    boot2docker down
    boot2docker destroy
    ````

    or if boot2docker from the command line is causing you problems, there is
    "Delete Boot2Docker VM" Start menu option which maps to delete.sh


    and to re-make the boot2docker-vm

    ````
    boot2docker init
    boot2docker up
    ````

    > On Windows `C:\Users\burr\.boot2docker` contain files associated with your installation and I have seen .boot2docker not be uninstalled properly, manual deletion may be necessary

Check out the follow-on tutorial for adding MySQL.
<https://github.com/burrsutter/docker_mysql_tutorial>
