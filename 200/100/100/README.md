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

We'll go ahead and create a working directory. 

```
$ cd ~
$ mkdir nrm3docker
```

I'll go ahead and CD into that directory and I'll create my Docker file.

```
$ cd onboarding
$ vim dockerfile
```

Now, you don't actually have to use the term Docker file because you can specify what file you're gonna use. However, by default, it looks for that Docker file. 

So the first line is gonna be that from command. Where are or what are we? What are we basing this image on?

```
FROM centos:centos7
```
dockerfile

*** WE ARE HERE ***


