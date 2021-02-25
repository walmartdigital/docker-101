## Overview

In this lab, you will run your first Docker container.

## Prerequisites

To complete Lab 01: You must have access to a docker client, either on localhost, use a terminal iTerm2 or be using Play with Docker for example.

## Duration

|Component       |Timing            |
|----------------|------------------|
|Overview        |1 minutes         |
|Prerequisites   |1 minutes         |
|Lab             |15 minutes        |

## Get Started

* Run
  ```
    docker -h
        Flag shorthand -h has been deprecated, please use --help

        Usage:  docker [OPTIONS] COMMAND

        A self-sufficient runtime for containers

        Options:
            --config string      Location of client config files (default "/root/.docker")
        -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context set with "docker context
                                use")
        -D, --debug              Enable debug mode
        -H, --host list          Daemon socket(s) to connect to
        -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
            --tls                Use TLS; implied by --tlsverify
            --tlscacert string   Trust certs signed only by this CA (default "/root/.docker/ca.pem")
            --tlscert string     Path to TLS certificate file (default "/root/.docker/cert.pem")
            --tlskey string      Path to TLS key file (default "/root/.docker/key.pem")
            --tlsverify          Use TLS and verify the remote
        -v, --version            Print version information and quit

        Management Commands:
        app*        Docker App (Docker Inc., v0.9.1-beta3)
        builder     Manage builds
        checkpoint  Manage checkpoints
        config      Manage Docker configs
        container   Manage containers
        context     Manage contexts
        image       Manage images
        manifest    Manage Docker image manifests and manifest lists
        network     Manage networks
        node        Manage Swarm nodes
        plugin      Manage plugins
        secret      Manage Docker secrets
        service     Manage services
        stack       Manage Docker stacks
        swarm       Manage Swarm
        system      Manage Docker
        trust       Manage trust on Docker images
        volume      Manage volumes

        Commands:
        attach      Attach local standard input, output, and error streams to a running container
        build       Build an image from a Dockerfile
        commit      Create a new image from a container's changes
        cp          Copy files/folders between a container and the local filesystem
        create      Create a new container
        diff        Inspect changes to files or directories on a container's filesystem
        events      Get real time events from the server
        exec        Run a command in a running container
        export      Export a container's filesystem as a tar archive
        history     Show the history of an image
        images      List images
        import      Import the contents from a tarball to create a filesystem image
        info        Display system-wide information
        inspect     Return low-level information on Docker objects
        kill        Kill one or more running containers
        load        Load an image from a tar archive or STDIN
        login       Log in to a Docker registry
        logout      Log out from a Docker registry
        logs        Fetch the logs of a container
        pause       Pause all processes within one or more containers
        port        List port mappings or a specific mapping for the container
        ps          List containers
        pull        Pull an image or a repository from a registry
        push        Push an image or a repository to a registry
        rename      Rename a container
        restart     Restart one or more containers
        rm          Remove one or more containers
        rmi         Remove one or more images
        run         Run a command in a new container
        save        Save one or more images to a tar archive (streamed to STDOUT by default)
        search      Search the Docker Hub for images
        start       Start one or more stopped containers
        stats       Display a live stream of container(s) resource usage statistics
        stop        Stop one or more running containers
        tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
        top         Display the running processes of a container
        unpause     Unpause all processes within one or more containers
        update      Update configuration of one or more containers
        version     Show the Docker version information
        wait        Block until one or more containers stop, then print their exit codes

        Run 'docker COMMAND --help' for more information on a command.
  ```
  
* To get more help with docker, check out [Guides](https://docs.docker.com/go/guides/)

* The Docker command line can be used to manage several features of the Docker Engine. In this lab, we will mainly focus on the +*container** command

* Open your terminal on your local computer or Login into the Play with Docker lab environment using your docker hub credentials.

## 1. Let's Run your first container!

* Run 
  ```
  docker container run -t ubuntu top   
    Unable to find image 'ubuntu:latest' locally
    latest: Pulling from library/ubuntu
    83ee3a23efb7: Pull complete 
    db98fc6f11f0: Pull complete 
    f611acd52c6c: Pull complete 
    Digest: sha256:703218c0465075f4425e58fac086e09e1de5c340b12976ab9eb8ad26615c3715
    Status: Downloaded newer image for ubuntu:latest
  ```

* It should look like this:
  
  ```
  [node1] (local) root@192.168.0.23 ~
  top - 21:55:12 up 57 days, 21:07,  0 users,  load average: 10.93, 10.17, 10.37
    Tasks:   1 total,   1 running,   0 sleeping,   0 stopped,   0 zombie
    %Cpu(s): 18.3 us, 28.2 sy,  0.0 ni, 52.2 id,  0.1 wa,  0.0 hi,  1.2 si,  0.0 st
    MiB Mem :  32174.7 total,   2911.9 free,  22183.9 used,   7078.9 buff/cache
    MiB Swap:      0.0 total,      0.0 free,      0.0 used.   9005.8 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
        1 root      20   0    6148   3204   2736 R   0.0   0.0   0:00.07 top
  ```

* Open a new terminal window or tab
  
* Using play-with-docker.com, to open a new terminal connected to node1, click "Add New Instance" on the lefthand side
  
* Then ssh from node2 into node1 using the IP that is listed by 'node1 '
  
  ```
  [node2] (local) root@192.168.0.22 ~
    $ ssh 192.168.0.23
    The authenticity of host '192.168.0.23 (192.168.0.23)' can't be established.
    RSA key fingerprint is SHA256:1GK08rjZ3N/bdG5XgMT0ERECdthc09muRMYHlzYEOdw.
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
    Warning: Permanently added '192.168.0.23' (RSA) to the list of known hosts.
    ###############################################################
    #                          WARNING!!!!                        #
    # This is a sandbox environment. Using personal credentials   #
    # is HIGHLY! discouraged. Any consequences of doing so are    #
    # completely the user's responsibilites.                      #
    #                                                             #
    # The PWD team.                                               #
    ###############################################################
    ```

* Run docker container ls command to get the ID of the running container you just created.
    
    ```    
    [node1] (local) root@192.168.0.23 ~
    $ docker container ls
    CONTAINER ID   IMAGE     COMMAND   CREATED              STATUS              PORTS     NAMES
    21827b06087e   ubuntu    "top"     About a minute ago   Up About a minute             xenodochial_margulis
    ```

* Then use that **container id** to run bash inside that container using the docker container exec command

* Since we are using bash and want to interact with this container from our terminal, use -it flags to run using interactive mode while allocating a psuedo-terminal.

    ```
    docker container exec -it 21827b06087e bash or **docker run exec -it 21827b06087e -- sh**
    root@21827b06087e:/# 
    ```

* From the same terminal, run ps -ef to inspect the running processes.

    ```
    root@21827b06087e:/# ps -ef
    UID        PID  PPID  C STIME TTY          TIME CMD
    root         1     0  0 22:25 pts/0    00:00:00 top
    root         8     0  0 22:34 pts/1    00:00:00 bash
    root        16     8  0 22:35 pts/1    00:00:00 ps -ef
    ```

* These commands will work on **linux or mac**. For windows, you can inspect the running processes using **tasklist**

    ```
    root@21827b06087e:/# exit
    exit
    $ ps -ef
    # Tons of processes!
    ```

* Let's clean up everything on the first terminal or first node of your Play with Docker tab close the container by typing <ctrl>-c.

* List all the containers and remove them by selecting their container id
 
    ```
    docker ps -a

    docker rm <CONTAINER ID>
    ```

## 2. Let's make it more fun and Run multiple Containers

* Explore the Docker Hub "The Docker Hub is the public central registry for Docker images, which contains community and official images"

* It is important to avoid using unverified content from the Docker Store when developing your own images that are intended to be deployed into the production environment

* These unverified images may contain security vulnerabilities or possibly even malicious software

* Run an Nginx server
    
    ```
    docker container run --detach --publish 8080:80 --name nginx nginx
    docker container run --detach --publish 8080:80 --name nginx nginx
    Unable to find image 'nginx:latest' locally
    latest: Pulling from library/nginx
    45b42c59be33: Pull complete 
    8acc495f1d91: Pull complete 
    ec3bd7de90d7: Pull complete 
    19e2441aeeab: Pull complete 
    f5a38c5f8d4e: Pull complete 
    83500d851118: Pull complete 
    Digest: sha256:f3693fe50d5b1df1ecd315d54813a77afd56b0245a404055a946574deb6b34fc
    Status: Downloaded newer image for nginx:latest
    939a79ba3cb2caa02231afb48b75aa54f0bfe6ae8868ed82a8ca8fbf4ab362a2
    [node2] (local) root@192.168.0.22 ~
    ```

* Let's access the nginx server on localhost:8080

    ```
    curl localhost:8080
    ```

* will return the HTML home page of Nginx

* If you are using play-with-docker, look for the 8080 link near the top of the page, or if you run a Docker client with access to a local browser

## Let's Run a Azure SQL Edge Server

    ## Prerequisites to deploy Azure SQL Edge Container

    * This image requires Docker Engine 1.8+ in any of their supported platforms.

    * At least 1GB of RAM. Make sure to assign enough memory to the Docker VM if you're running on Docker for Mac or Windows.

    * Requires the following environment flags

    ```
    ACCEPT_EULA=Y
    ```
    
    ```
    MSSQL_SA_PASSWORD=<your_strong_password>
    ```

    ```
    MSSQL_PID=<edition_name> (default: Developer)
    ```

    * A strong system administrator (SA) password: At least 8 characters including uppercase, lowercase letters, base-10 digits and/or non-alphanumeric symbols.

* Now, run a Azure SQL Edge Server using the latest official image [the azure sql edge docker hub page](https://hub.docker.com/_/microsoft-azure-sql-edge)

    ```
    docker pull mcr.microsoft.com/azure-sql-edge:latest
    ```

* Start a Azure SQL Edge instance running as the Developer edition

    ```
    docker run --cap-add SYS_PTRACE -e 'ACCEPT_EULA=1' -e 'MSSQL_SA_PASSWORD=yourStrong(!)Password' -p 1433:1433 --name azuresqledge -d mcr.microsoft.com/azure-sql-edge
    ```

* For a complete list of all Azure SQL Edge environment variable, see Configure [Azure SQL Edge with Environment Variables](https://docs.microsoft.com/en-us/azure/azure-sql-edge/configure#configure-by-using-environment-variables).You can also use a [mssql.conf](https://docs.microsoft.com/en-us/azure/azure-sql-edge/configure#configure-by-using-an-mssqlconf-file) file to configure Azure SQL Edge Containers.

* To view your Docker containers, use the docker ps command

    ```
    docker ps -a
    ```

* If the STATUS column shows a status of Up, then Azure SQL Edge is running in the container and listening on the port specified in the PORTS column. If the STATUS column for your Azure SQL Edge container shows Exited, see the Troubleshooting section of Azure SQL Edge Documentation.

* The -h (host name) parameter is also useful, but it is not used in this tutorial for simplicity. This changes the internal name of the container to a custom value. This is the name you'll see returned in the following Transact-SQL query:

    ```
    SELECT @@SERVERNAME,
    SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),
    SERVERPROPERTY('MachineName'),
    SERVERPROPERTY('ServerName')
    ```

* Setting -h and --name to the same value is a good way to easily identify the target container 

* As a final step, change your SA password because the SA_PASSWORD is visible in ps -eax output and stored in the environment variable of the same name. See steps below

## Change the SA password

* The SA account is a system administrator on the Azure SQL Edge instance that gets created during setup. After creating your Azure SQL Edge container, the MSSQL_SA_PASSWORD environment variable you specified is discoverable by running echo $SA_PASSWORD in the container. For security purposes, change your SA password

* Choose a strong password to use for the SA user

* Run

    ```
    docker exec -it azuresqledge /opt/mssql-tools/bin/sqlcmd \
   -S localhost -U SA -P "<YourStrong@Passw0rd>" \
   -Q 'ALTER LOGIN SA WITH PASSWORD="<YourNewStrong@Passw0rd>"'
    ```

## Let's Connect to Azure SQL Edge

* User the docker exec -it command to start an interactive bash shell inside your running container

    ```
    docker exec -it --name azuresqledge "bash"
    ```

* Once inside the container, connect locally with sqlcmd. Sqlcmd is not in the path by default, so you have to specify the full path

    ```
    /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "<YourNewStrong@Passw0rd>"
    ```

## Create and query data

* Using the example from [Azure SQL Edge Docker Container](https://docs.microsoft.com/en-us/azure/azure-sql-edge/disconnected-deployment#create-and-query-data)

## Connect from outside the container

* Using the example from [Connect from outside the container](https://docs.microsoft.com/en-us/azure/azure-sql-edge/disconnected-deployment#connect-from-outside-the-container)

## Let's remove your container

* To remove the container of the Azure SQL Edge use the following commands

    ```
     docker stop azuresqledge
     docker rm azuresqledge
    ```

* Stopping and removing a container permanently deletes any Azure SQL Edge data in the container. If you need to preserve your data, create and copy a backup file out of the container or use a container data persistence technique

## Summary

In this lab you learned to create your first Docker Container using docker hub official images