# 100 Installing, Starting, and Shutting Down Nexus Repository

## 000 Prerequisites

- Docker is installed, see https://github.com/vanHeemstraSystems/docker-quick-start-headstart

## 100 Installing Docker Image of Nexus

So in order to see if we have any images there, we can use the command docker images.

```
$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
```

You see that we currently have no images available.

What we're gonna do is we're gonna create our own image.

We'll go ahead and create a working directory (here named: nrm3docker for Nexus repository Manager version 3 with Docker). 

```
$ cd ~
$ mkdir nrm3docker
```

I'll go ahead and CD into that directory and I'll create my Docker file.

```
$ cd nrm3docker
$ vim dockerfile
```

Now, you don't actually have to use the term Docker file because you can specify what file you're gonna use. However, by default, it looks for that Docker file. 

So the first line is gonna be that from command. Where are or what are we? What are we basing this image on?

```
FROM centos:centos7
```
dockerfile

Then we'll go ahead and do an update and will install Python three. 

| | NRM3 | |
| -- | -- | -- |
| | yum -y install python3 | |
| | yum -y update | |
| | Base image: CentOS 7 | |

vanheemstrasystems/nrm3

Use Best Practices as can be found at https://beenje.github.io/blog/posts/dockerfile-anti-patterns-and-best-practices/

Update the Docker file for the above. Add that -y flag in so that we're not prompted to accept the installation of Python 3.

```
FROM centos:centos7
RUN yum -y update
RUN yum -y install python3
```
dockerfile

We want to have that maintainer status, so simply go ahead and use label as your instruction.

```
FROM centos:centos7
RUN yum -y update
RUN yum -y install python3
LABEL maintainer="willem@vanheemstrasystems.com"
```
dockerfile

Then we can use the command docker build.

```
$ docker build dockerfile
```

Be building the image and I could give it the name of the file on many years or more commonly, you'll see a dot that means build using the docker filed that is in this current directory.

```
$ docker build .
```

So we see our build context, which is what we wrote in that file being sent to the Docker daemon.

*** WE ARE HERE ***


