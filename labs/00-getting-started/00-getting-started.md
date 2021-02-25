# Basic Setup
The lab is divided in two parts. One that uses Docker Desktop, a tool that doesn't need to fiddle with VMs or add a bunch of extra components; simply install from a single package and have your first containers running in minutes. The second part will have you deploy your basics docker commands.

The exercises in this lab will have you use a variety of tools that you should install before attending the workshop. This guide attempts to provide instructions on how to install these tools for MacOs and Homebrew users.

## Prerequisites
Before attempting to clone repo, be sure your host machine meets the prerequisites:

* git
* [Homebrew](https://brew.sh)
* Check the versions of Docker Engine,Compose,and Machine [Docker Getting Started](http://docs.docker.oeynet.com/docker-for-mac/)
* curl

## Duration

|Component       |Timing            |
|----------------|------------------|
|Basics          |3 minutes         |
|Prerequisites   |5 minutes         |
|Lab             |5 minutes         |

## Docker Getting Started
```
    docker --version
    docker-compose --version
    docker-machine --version
```
## Install curl
```
    brew install curl
```

## Clone the project from Github
```
    git clone https://github.com/walmartdigital/docker-101.git
```

Now you can go to [Creating your first container](https://github.com/walmartdigital/docker-101/blob/master/labs/01-creating-your-fist-container/01-creating-your-first-container.md)

## Login into Play with Docker using your docker hub account
```
    https://labs.play-with-docker.com/
    write your username or email 
    write your docker hub pass
```

## Summary

In this lab you learned to create your Docker Hub account run basic docker commands to validate docker CLI capabilities.
