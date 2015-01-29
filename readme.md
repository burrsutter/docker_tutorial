Docker with boot2docker for Windows Tutorial
============================================

This tutorial walks you through the basics of using a Java app server (WildFly) via a Linux container, running on Windows with boot2docker.  There are notes for people running on Macs as well.  

First follow [installation steps](https://docs.docker.com/installation/windows/) for boot2docker:

![Alt text](/screenshots/installer.png?raw=true "Installer")

Unless you already have VirtualBox installed, install all the components.

> Mac: The docker and boot2docker binaries are in `/usr/local/bin` which you can access from your terminal.
> Windows: The boot2docker binary lands in `C:\Program Files\Boot2Docker` for Windows

* * *
##### Tip 1: where does the boot2docker VM (ISO) land on a Windows?*
Windows: `C:\Users\Burr\\.boot2docker\boot2docker.iso`
Mac: `~/.boot2docker/boot2docker.iso`
* * *

##### Tip 2: where does the boot2docker instance land on Windows installation of VirtualBox

    D:\Users\Burr\.VirtualBox\Machines\boot2docker-vm\Snapshots

> I have virtual box using a different drive on my machine

* * *
##### Tip 3: Window Size Width 160 - docker ps is best displayed with lots of width

![Alt text](/screenshots/cmd_properties.png?raw=true "cmd.exe Properties")
* * *

##### Tip 4: VirtualBox before `boot2docker init`
![Alt text](/screenshots/virtualbox_before_boot2docker.png?raw=true "VirtualBox Before")
> VirtualBox installed prior to boot2docker has no mention of boot2docker until we use `boot2docker init`.
> Also, if you have previously installed boot2docker, you can often use `boot2docker upgrade` to simply update to the latest version.
* * *

#### Explore Docker

1. Open a command prompt aka "cmd.exe" and type `boot2docker version`

    ![Alt text](/screenshots/boot2docker_version.png?raw=true "Command Prompt boot2docker version")
    > We have seen this fail on some systems.  There will be some workarounds listed in the steps below.

2. `boot2docker init`
    ![Alt text](/screenshots/boot2docker_init.png?raw=true "boot2docker init")

    > You should see the boot2docker-vm listed in VirtualBox Manager

    ![Alt text](/screenshots/virtualbox_after_boot2docker.png?raw=true "VirtualBox After")

3. `boot2docker up`

    Watch for the following on _Mac OSX_:

    ![Alt text](/screenshots/macosx_env_vars.png?raw=true "Mac OSX Env Vars")

    Copy and paste the export statements you are provided with in to your terminal. If you get this step wrong, when you try further commands, you may see an error message like:

    ![Alt text](/screenshots/failed_macosx_env_vars.png?raw=true "Fail Mac OSX Env Vars")

    Look for the following on _Windows_:

    ![Alt text](/screenshots/after_boot2docker_up.png?raw=true "boot2docker up")

    > Now, if you check the VirtualBox GUI you will see the `boot2docker-vm` running:

    ![Alt text](/screenshots/after_boot2docker_up_virtual_box.png?raw=true "boot2docker up results in VirtualBox")

    > Watch out for "VT-x is disabled in BIOS" errors
    > If virtualization has not been enabled in your machines BIOS, you could see the following error:

    ![Alt text](/screenshots/virtualization_error.png?raw=true "virtualization error")

    > If you see this error, you will need to update your BIOS settings accordingly.  

    > Also watch out for
    >
    > Error requesting socket: exec: "ssh": executable file not found in %PATH%
    >
    > The workaround here is to use start.sh to launch the command prompt (not the normal Windows command prompt)
    >
    > You should be able to double-click on start.sh in C:\Program Files\Boot2Docker for Windows

    ![Alt text](/screenshots/boot2docker_start_sh.png?raw=true "start.sh")

    > If you successfully launch start.sh, it will execute `up`, `status` and `ip`, therefore you can skip to step 7 below.
    > Do make note of the IP address that is printed out, you will need it later.

    ![Alt text](/screenshots/start_sh_running.png?raw=true "start.sh running")


4. `boot2docker status`
    > When it is time to shutdown, run `boot2docker down`

5. `boot2docker ip`
    ![Alt text](/screenshots/boot2docker_ip.png?raw=true "boot2docker ip")

    > You will need this later!
6. `boot2docker ssh`
    ![Alt text](/screenshots/boot2docker_ssh.png?raw=true "boot2docker ssh")

    > From this point forward, you will be inside of a Linux shell, using Linux commands

7. `docker version`

8. `docker info`
    ![Alt text](/screenshots/docker_info.png?raw=true "docker info")

9. `docker`
    ![Alt text](/screenshots/docker_lists_commands.png?raw=true "docker info")

10. `docker images`

11. `docker ps -a`
    ![Alt text](/screenshots/docker_ps_a.png?raw=true "docker ps -a")

12. `docker run centos /bin/echo "Hello World"`

    ![Alt text](/screenshots/docker_run_centos.png?raw=true "docker run centos")

    This will take some time if this is the first run of the "centos" image
    If you run the same command again, you will notice that is runs immediately, no download required.
    A Docker container starts incredibly fast when compared to traditional virtual machine technology

13. On Windows, with boot2docker 1.3.x, the Users directory is shared as `/c/Users`

    `ls /c/Users`
    ![Alt text](/screenshots/ls_users.png?raw=true "ls /c/Users")

    This shared folder will allow you to add and edit files using your traditional Windows tools instead of having to learn vi or nano.

    Create a `demo` sub-directory to your home directory and then use a `ls -l` to see it via the boot2docker-vm (in this example `Burr` is the username):

    `ls -l /c/Users/Burr/demo`

    ![Alt text](/screenshots/ls_l_c_users_Burr_demo.png?raw=true "ls -l /c/Users/Burr/demo")

    In this example, there are already some sample projects:

    ![Alt text](/screenshots/windows_explorer.png?raw=true "Windows Explorer")

14. `docker run -i -t centos /bin/bash`
    ````
    -i interactive
    -t allows your keyboard input
    ````

    You can also use `-it` as well as `-i -t`.  Remember this trick - if you have an app server failing to start,  you can see the console output and review the logs by using "-it"

    `cat /etc/system-release`

    ![Alt text](/screenshots/cat_etc_system_release.png?raw=true "cat /etc/system-release")

    type `exit` to leave the container and drop back into the boot2docker-vm shell.

15. `docker ps`

    There should be no currently running containers since `exit` terminated the centos container
    ![Alt text](/screenshots/docker_ps.png?raw=true "docker ps")

16. `docker ps -a`

    but there have been previously run containers

    ![Alt text](/screenshots/docker_ps_a_2.png?raw=true "docker ps -a")

17. `docker images`

    shows local images
    ![Alt text](/screenshots/docker_images.png?raw=true "docker images")

18. `docker pull centos/wildfly`

    Docker Hub contains a large number of pre-configured images that are ready to use via a simple "pull" e.g. <https://registry.hub.docker.com/u/centos/wildfly/>

    > `run` does an implicit "pull" if the image is not already downloaded

    Docker images are typically identified by two words `"owner"/"imagename"`
    The `centos/wildfly` image includes nice documentation on how to use it - we will be following several of those steps next.

    ![Alt text](/screenshots/docker_pull_centos_wildfly.png?raw=true "docker pull centos/wildfly")

19. `docker run -it centos/wildfly`

    ![Alt text](/screenshots/docker_run_centos_wildfly.png?raw=true "docker run -it centos/wildfly")

    The `t` is important so you can `Ctrl-C` to stop wildfly and the container.

    Hit `Ctrl-C` and run a `docker ps` to see that the container has been stopped.

    In this particular case, the WildFly instance does not expose any ports to the outside world, let's try that next.

20. `docker run -it -p 8080:8080 centos/wildfly`

    ![Alt text](/screenshots/docker_run_it_p_8080_8080.png?raw=true "docker run -it -p 8080:8080 centos/wildfly")

    Now, if you remember the IP address (from `boot2docker ip`) you can use your favorite browser to hit the server

    ![Alt text](/screenshots/browser_wildfly.png?raw=true "http://192.168.59.103:8080")

    If you have forgotten your IP address, just open another Command Prompt and type "boot2docker ip"
    ![Alt text](/screenshots/boot2docker_ip_2nd_command_prompt.png?raw=true "boot2docker ip")

    Press `Ctrl-C` to terminate the WildFly container.

21. `docker history centos/wildfly`

    The history command allows you to see more detail into how the image was crafted
    ![Alt text](/screenshots/docker_history_centos_wildfly.png?raw=true "docker history centos/wildfly")

#### Modify the image and provide our own custom Java application

1. If you remember way back to `ls /c/Users/Burr/demo`, the `Users` directory on your Windows host
is shared with the boot2docker-vm (thanks to VirtualBox Guest Additions). In your home directory, create a directory called `docker_projects` that is a sibling of `demo`.

    ````
    mkdir /c/Users/Burr/docker_projects
    ````
    > Use your home directory name in place of "Burr"

    and then create a sub-directory called `myapp`

    ````
    mkdir /c/Users/Burr/docker_projects/myapp
    ````

    ![Alt text](/screenshots/windows_explorer_myapp.png?raw=true "Windows Explorer myapp")

    and then change to the directory

    ````
    cd /c/Users/Burr/docker_projects/myapp
    ````

2. In the `myapp` directory, create a text file called `Dockerfile`, with no extension.

    > On Windows we recommend using Notepad++ for text editing.

    ![Alt text](/screenshots/dockerfile_windows_explorer.png?raw=true "Windows Explorer Dockerfile")


3.  Edit the newly created `Dockerfile` and add the following two lines:

    ````
    FROM centos/wildfly

    ADD html5java.war /opt/wildfly/standalone/deployments/
    ````

    > The trailing "/" does matter

    You can find `html5java.war` at <https://github.com/burrsutter/docker_tutorial/blob/master/html5java.war?raw=true>. Download the war and copy it to the `myapp` directory.

4. Back in the boot2docker ssh session

    ````
    docker build --tag=myapp .
    ````

    > the trailing "." is important

    ![Alt text](/screenshots/docker_build.png?raw=true "docker build")

5. Let's see if that worked

    ````
    docker run -it -p 8080:8080 myapp
    ````

    you should see the deployment of `html5java.war` in the wildfly console logging

    ![Alt text](/screenshots/html5java_war_deployment.png?raw=true "docker run -it -p 8080:8080 myapp")

6. And test the app via your browser <http://192.168.59.103:8080/html5java>

    ![Alt text](/screenshots/browser_html5java_myapp.png?raw=true "http://192.168.59.103:8080/html5java")

#### Cleanup

OPTIONAL - Clean Slate: If you wish to completely clean up and run through the above steps again:

1. Remove/Delete all containers

    ````
    docker rm \`docker ps -a -q\`
    ````

    >  the back ticks are important!

    You might also need to "stop" or "kill" any containers that are running and will not remove.

    ````
    docker ps
    docker stop CONTAINER_ID
    docker kill CONTAINER_ID
    ````

    Replace CONTAINER_ID with the id seen in the `docker ps` results.

2. Remove/Delete all images

    ````
    docker rmi \`docker images -a -q\`
    ````

    > watch those back ticks again

3. Exit the boot2docker-vm shell, back at the Windows Command Prompt

    ````
    boot2docker down
    boot2docker destroy
    ````

    and to re-make the boot2docker-vm

    ````
    boot2docker init
    boot2docker up
    ````

    > On Windows `C:\Users\burr\\.boot2docker` contain files associated with your installation and I have seen .boot2docker not be uninstalled properly, manual deletion may be necessary

The End (for now)
