## Overview

In this lab, you will create, build and push your first Docker image and Dockerfile

## Duration

|Component       |Timing            |
|----------------|------------------|
|Overview        |1 minutes         |
|Prerequisites   |1 minutes         |
|Lab             |15 minutes        |


## Prerequisites

To complete the lab 02: Must have access to docker client, docker hub account, terminal console with at least 2 different sessions or you can use Play with Docker [PWD](https://labs.play-with-docker.com/), important you can also use docker minimal images.

To Understand what's a Dockerfile remember to review the [Slide] ()

## Let's Get Started

* Run on your terminal or in your [PWD](https://labs.play-with-docker.com/) the following command

        ```
        mkdir testimage
        cd testimage
        vim Dockerfile
        ```

* Within that Dockerfile let's understand layering using a single stage example, please copy the information bellow to into the de Dockerfile

        ```
        FROM ubuntu:latest
        RUN mkdir -p /hello/hello
        COPY hello.txt /hello/hello
        RUN chmod 600 /hello/hello/hello.txt

        * Save it and let's build a cool hello.txt

        vim hello.txt
        Hello! We come in peace

        * Remember to save your changes and let's build a docker container using this awesome Dockerfile

        docker build -t testimage .
        Sending build context to Docker daemon  3.072kB
        Step 1/4 : FROM ubuntu:latest
        ---> f63181f19b2f
        Step 2/4 : RUN mkdir -p /hello/hello
        ---> Using cache
        ---> 3b81d0784347
        Step 3/4 : COPY hello.txt /hello/hello
        ---> 1063b44840f3
        Step 4/4 : RUN chmod 600 /hello/hello/hello.txt
        ---> Running in 37e57880b77b
        Removing intermediate container 37e57880b77b
        ---> f490ff9f5356
        Successfully built f490ff9f5356
        Successfully tagged testimage:latest

        ```

* To get to know what's running on your docker environment run.

        ```
        docker history testimage
        IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
        f490ff9f5356   4 minutes ago   /bin/sh -c chmod 600 /hello/hello/hello.txt     24B
        1063b44840f3   4 minutes ago   /bin/sh -c #(nop) COPY file:8a58dd09935078ae…   24B
        ```


* Just kidding this was only a tiny example so move up to next level.

        ```
        # Replace latest with a pinned version tag from https://hub.docker.com/_/alpine
        #
        # We suggest using the major.minor tag, not major.minor.patch, since you can be exposed to security vulnerabilities 
        FROM alpine:latest

        # Important to establish a maintainer information with the creator or team creator in order to keep the tracking of releases 
        LABEL maintainer=<Infinitus-tribe-email>
        LABEL version="1.0"

        # Non-root user for security purposes.
        #
        # UIDs below 10,000 are a security risk, as a container breakout could result
        # in the container being ran as a more privileged user on the host kernel with
        # the same UID.
        #
        # Static GID/UID is also useful for chown'ing files outside the container where
        # such a user does not exist.
        RUN addgroup -g 10001 -S <YOURNONROOTUSER> && adduser -u 10000 -S -G <YOURNONROOTUSER> -h /home/nonroot nonroot

        # Install packages here with `apk add --no-cache`, copy your binary
        # into /sbin/, etc.

        # Tini allows us to avoid several Docker edge cases, see https://github.com/krallin/tini.
        RUN apk add --no-cache tini
        # Install a group of packages to ease the administration in order to compress the docker image size
        RUN \
        apk update && \
        apk add bash py-pip curl gnupg && \
        pip install --upgrade pip && \
        apk add --virtual=build gcc libffi-dev musl-dev openssl-dev python-dev make && \
        pip install azure-cli && \
        apk del --purge build 

        ENTRYPOINT ["/sbin/tini", "--", "myapp"]
        # Replace "myapp" above with your binary

        # bind-tools is needed for DNS resolution to work in *some* Docker networks, but not all.
        # This applies to nslookup, Go binaries, etc. If you want your Docker image to work even
        # in more obscure Docker environments, use this.
        RUN apk add --no-cache bind-tools

        # Use the non-root user to run our application
        USER <YOURNONROOTUSER>

        # Default arguments for your app (remove if you have none):
        CMD ["--foo", "1", "--bar=2"]

        # Let's expose the docker container in a port
        EXPOSE 3000 
        ```

* Let's deep dive into our Dockerfile and start to build the image

    + Each Dockerfile has a LayerID which will help you out to understand the read/write instructions let's inspect the Docker image

    ```
        docker inspect testimage:latest
    ```

* The result let's review locally to avoid huge code lines in this Workshop

    ```json
        "DockerVersion": "20.10.2",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "sha256:1063b44840f3d961095da676141f6b7706cebc416dcd7d75327a1a30e7c3378f",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 72901328,
        "VirtualSize": 72901328,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/ab2a783ed494032e85fca28b8ed72e1cd823966063bdaf9095759cc383a6880c/diff:/var/lib/docker/overlay2/a30354dfc43737c890a7b0ef4cfb21f18358b95964549da1fef2409ce624ab76/diff:/var/lib/docker/overlay2/cd1abfafc6566b05b84b4d6c35b2f053e9ddf80248f24eec0185fa25f48adb8e/diff:/var/lib/docker/overlay2/c26cdb855b6bd6bb56a9733c6bca66031a074a800fe42263f987b00fb65e4014/diff:/var/lib/docker/overlay2/367eb24e4ed0502a18e68581e309b4c27632f297ddcc82fe3608dc676adfd5a9/diff",
                "MergedDir": "/var/lib/docker/overlay2/a9d4596a05ffcf3335953b743360217e0cff58375f55d6f63327ff967d81f246/merged",
                "UpperDir": "/var/lib/docker/overlay2/a9d4596a05ffcf3335953b743360217e0cff58375f55d6f63327ff967d81f246/diff",
                "WorkDir": "/var/lib/docker/overlay2/a9d4596a05ffcf3335953b743360217e0cff58375f55d6f63327ff967d81f246/work"
    ```

* As you can see in your local terminal or in your [PWD](https://labs.play-with-docker.com/), you're getting the specs of your docker container image

* To understand why Dockerfiles instructions gives a wide range of options you need to undertstand the origin and baseline of the docker image.

* Each line on your first exercise RUN a desired state command and it'll trigger the call to reach their sources it can be a folder, a file, a batch process the limits are none

* Being said that you need to keep isolated your docker container starting applying the best practices that you can get them from the second Dockerfile presented
  
```
        # Replace latest with a pinned version tag from https://hub.docker.com/_/alpine
        #
        # We suggest using the major.minor tag, not major.minor.patch, since you can be exposed to security vulnerabilities 
        FROM alpine:latest #Use official certified images or tested images avoid to let loose security constraints and keep it secure
        # Declare your The WORKDIR directive in Dockerfile defines the working directory for the rest of the instructions in the Dockerfile
        WORKDIR /myapp
        RUN echo "Welcome to Docker Labs" > opt.txt
        WORKDIR folder1
        RUN echo "Welcome to Docker Labs" > folder1.txt
        WORKDIR folder2
        RUN echo "Welcome to Docker Labs" > folder2.txt
        # Important to establish a maintainer information with the creator or team creator in order to keep the tracking of releases 
        LABEL maintainer=<Infinitus-tribe-email>
        LABEL version="1.0"

        # Non-root user for security purposes.
        #
        # UIDs below 10,000 are a security risk, as a container breakout could result
        # in the container being ran as a more privileged user on the host kernel with
        # the same UID.
        #
        # Static GID/UID is also useful for chown'ing files outside the container where
        # such a user does not exist.
        RUN addgroup -g 10001 -S <YOURNONROOTUSER> && adduser -u 10000 -S -G <YOURNONROOTUSER> -h /home/nonroot nonroot

        # Install packages here with `apk add --no-cache`, copy your binary
        # into /sbin/, etc.

        # Tini allows us to avoid several Docker edge cases, see https://github.com/krallin/tini.
        RUN apk add --no-cache tini
        # Install a group of packages to ease the administration in order to compress the docker image size
        RUN \
        apk update && \
        apk add bash py-pip curl gnupg && \
        pip install --upgrade pip && \
        apk add --virtual=build gcc libffi-dev musl-dev openssl-dev python-dev make && \
        pip install azure-cli && \
        apk del --purge build 

        ENTRYPOINT ["/sbin/tini", "--", "myapp"]
        # Replace "myapp" above with your binary

        # bind-tools is needed for DNS resolution to work in *some* Docker networks, but not all.
        # This applies to nslookup, Go binaries, etc. If you want your Docker image to work even
        # in more obscure Docker environments, use this.
        RUN apk add --no-cache bind-tools

        # Use the non-root user to run our application
        USER <YOURNONROOTUSER>

        # Default arguments for your app (remove if you have none):
        CMD ["--foo", "1", "--bar=2"]

        # Let's expose the docker container in a port
        EXPOSE 3000 
```

* Let's create another folder in another path of the testimage exercise, and a new Dockerfile with the last format provided.

```
        mkdir Dockertest
        cd Dockertest
        vim Dockerfile
        <Paste the Dockerfile refered before here and Save your changes>
```

* Important change the user name to be assigned to execute the container **This lines** 

```
        RUN addgroup -g 10001 -S <YOURNONROOTUSER> && adduser -u 10000 -S -G <YOURNONROOTUSER> -h /home/nonroot nonroot
        ## This one too.
        USER <YOURNONROOTUSER>
        LABEL maintainer=<Infinitus-tribe-email>
        LABEL version="1.0"
```

* Now let's build an image with this Dockerfile
  
```
        docker build -t Dockertest .
```

* View the result and remember you can add RUN COPY ADD and CMD (it'll count only the last CMD on the file as a valid step) to enhance the configuration.

* Let's run again the command to get the status

```
        docker history Dockertest
        IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
        f4534ff9f2342   8 minutes ago   /sbin/tini -c chmod infinitus 600 /sbin/tini/myapp     96M
        1063b44840f3   8 minutes ago   /sbin/tini -c #(nop) COPY /myapp file:5a58dd498672ae…   96M
```

* Now you know that your user has advanced privileges and can run instructions without being part or be root

* As for now you can use the Docker Cheat Sheet to play around [DockerCS](https://github.com/walmartdigital/docker-101/blob/master/DockerCommands101.md)

## Summary

In this lab you learned to build your first Docker Container using Dockerfile and official images