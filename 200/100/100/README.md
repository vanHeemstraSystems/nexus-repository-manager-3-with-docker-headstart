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

**Red Hat Universal Base Image (UBI)**

The recently released Red Hat Universal Base Image (UBI) provides a freely redistributable subset of Red Hat Enterprise Linux (RHEL). The UBI will be the only available base image selection for RHEL 8, however there is also a version of UBI for RHEL 7, provided as an alternative to the pre-UBI image. See also https://redhat-connect.gitbook.io/best-practices-guide/base-image

**WARNING**: All RHEL container images, including UBI-based images containing packages from RHEL repositories, are ***non-redistributable***. This means that they can't be hosted from third party or public container registries such as Docker Hub or Quay.io, or from a self-hosted registry. 

As we won't be distributing publicly we are not blocked by this limitation.

So the first line is gonna be that from command. Where are or what are we? What are we basing this image on?

We are going to use the following base image (see also https://www.redhat.com/en/blog/introducing-red-hat-universal-base-image):

UBI Init (systemd) registry.access.redhat.com/ubi8/ubi-init

```
# UBI 8 Init (systemd) image
FROM registry.access.redhat.com/ubi8/ubi-init
```
dockerfile

Then we'll go ahead and do an update. 

| | NRM3 | |
| -- | -- | -- |
| | yum -y update | |
| | Base image: RHEL8 UBI Init | |

vanheemstrasystems/nrm3

Use Best Practices as can be found at https://beenje.github.io/blog/posts/dockerfile-anti-patterns-and-best-practices/

Update the Docker file for the above. Add that -y flag in so that we're not prompted to accept the update.

```
# UBI 8 Init (systemd) image
FROM registry.access.redhat.com/ubi8/ubi-init

RUN yum -y update
```
dockerfile

We want to have that maintainer status, so simply go ahead and use label as your instruction.

```
# UBI 8 Init (systemd) image
FROM registry.access.redhat.com/ubi8/ubi-init

RUN yum -y update

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

Outcome of above build command:

```
Sending build context to Docker daemon  200.2kB
Step 1/3 : FROM registry.access.redhat.com/ubi8/ubi-init
latest: Pulling from ubi8/ubi-init
d9e72d058dc5: Pull complete 
cca21acb641a: Pull complete 
8d11216d378e: Pull complete 
Digest: sha256:c6d1e50ab1166beae21a334156091024aa9123059047bb7b51248a10d5168719
Status: Downloaded newer image for registry.access.redhat.com/ubi8/ubi-init:latest
 ---> 465c12d943fa
Step 2/3 : RUN yum -y update
 ---> Running in f55092d3d568
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.

Red Hat Universal Base Image 8 (RPMs) - BaseOS  4.1 MB/s | 773 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - AppStre  21 MB/s | 4.9 MB     00:00    
Red Hat Universal Base Image 8 (RPMs) - CodeRea 116 kB/s |  13 kB     00:00    
Dependencies resolved.
================================================================================
 Package        Architecture   Version               Repository            Size
================================================================================
Upgrading:
 tzdata         noarch         2021a-1.el8           ubi-8-baseos         473 k

Transaction Summary
================================================================================
Upgrade  1 Package

Total download size: 473 k
Downloading Packages:
tzdata-2021a-1.el8.noarch.rpm                   4.3 MB/s | 473 kB     00:00    
--------------------------------------------------------------------------------
Total                                           4.2 MB/s | 473 kB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1 
  Upgrading        : tzdata-2021a-1.el8.noarch                              1/2 
  Cleanup          : tzdata-2020d-1.el8.noarch                              2/2 
  Verifying        : tzdata-2021a-1.el8.noarch                              1/2 
  Verifying        : tzdata-2020d-1.el8.noarch                              2/2 
Installed products updated.

Upgraded:
  tzdata-2021a-1.el8.noarch                                                     

Complete!
Removing intermediate container f55092d3d568
 ---> a66e6c6dfa6e
Step 3/3 : LABEL maintainer="willem@vanheemstrasystems.com"
 ---> Running in fc32160def54
Removing intermediate container fc32160def54
 ---> ef6932d2f517
Successfully built ef6932d2f517
```

Check the status of the docker images.

```
$ docker images
REPOSITORY                                 TAG       IMAGE ID       CREATED              SIZE
<none>                                     <none>    ef6932d2f517   About a minute ago   245MB
registry.access.redhat.com/ubi8/ubi-init   latest    465c12d943fa   6 weeks ago          216MB
hello-world                                latest    bf756fb1ae65   13 months ago        13.3kB
```


*** WE ARE HERE ***








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

Outcome of above build command:

```
Sending build context to Docker daemon  200.2kB
Step 1/4 : FROM centos:centos7
 ---> 8652b9f0cb4c
Step 2/4 : RUN yum -y update
 ---> Running in dae8fb127c06
Loaded plugins: fastestmirror, ovl
Determining fastest mirrors
 * base: d36uatko69830t.cloudfront.net
 * extras: d36uatko69830t.cloudfront.net
 * updates: d36uatko69830t.cloudfront.net
Resolving Dependencies
--> Running transaction check
---> Package bind-license.noarch 32:9.11.4-26.P2.el7 will be updated
---> Package bind-license.noarch 32:9.11.4-26.P2.el7_9.3 will be an update
---> Package centos-release.x86_64 0:7-9.2009.0.el7.centos will be updated
---> Package centos-release.x86_64 0:7-9.2009.1.el7.centos will be an update
---> Package coreutils.x86_64 0:8.22-24.el7 will be updated
---> Package coreutils.x86_64 0:8.22-24.el7_9.2 will be an update
---> Package curl.x86_64 0:7.29.0-59.el7 will be updated
---> Package curl.x86_64 0:7.29.0-59.el7_9.1 will be an update
---> Package device-mapper.x86_64 7:1.02.170-6.el7 will be updated
---> Package device-mapper.x86_64 7:1.02.170-6.el7_9.3 will be an update
---> Package device-mapper-libs.x86_64 7:1.02.170-6.el7 will be updated
---> Package device-mapper-libs.x86_64 7:1.02.170-6.el7_9.3 will be an update
---> Package glib2.x86_64 0:2.56.1-7.el7 will be updated
---> Package glib2.x86_64 0:2.56.1-8.el7 will be an update
---> Package kpartx.x86_64 0:0.4.9-133.el7 will be updated
---> Package kpartx.x86_64 0:0.4.9-134.el7_9 will be an update
---> Package libcurl.x86_64 0:7.29.0-59.el7 will be updated
---> Package libcurl.x86_64 0:7.29.0-59.el7_9.1 will be an update
---> Package openssl-libs.x86_64 1:1.0.2k-19.el7 will be updated
---> Package openssl-libs.x86_64 1:1.0.2k-21.el7_9 will be an update
---> Package python.x86_64 0:2.7.5-89.el7 will be updated
---> Package python.x86_64 0:2.7.5-90.el7 will be an update
---> Package python-libs.x86_64 0:2.7.5-89.el7 will be updated
---> Package python-libs.x86_64 0:2.7.5-90.el7 will be an update
---> Package systemd.x86_64 0:219-78.el7 will be updated
---> Package systemd.x86_64 0:219-78.el7_9.2 will be an update
---> Package systemd-libs.x86_64 0:219-78.el7 will be updated
---> Package systemd-libs.x86_64 0:219-78.el7_9.2 will be an update
---> Package tzdata.noarch 0:2020d-2.el7 will be updated
---> Package tzdata.noarch 0:2020f-1.el7 will be an update
---> Package vim-minimal.x86_64 2:7.4.629-7.el7 will be updated
---> Package vim-minimal.x86_64 2:7.4.629-8.el7_9 will be an update
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================
 Package               Arch      Version                       Repository  Size
================================================================================
Updating:
 bind-license          noarch    32:9.11.4-26.P2.el7_9.3       updates     91 k
 centos-release        x86_64    7-9.2009.1.el7.centos         updates     27 k
 coreutils             x86_64    8.22-24.el7_9.2               updates    3.3 M
 curl                  x86_64    7.29.0-59.el7_9.1             updates    271 k
 device-mapper         x86_64    7:1.02.170-6.el7_9.3          updates    297 k
 device-mapper-libs    x86_64    7:1.02.170-6.el7_9.3          updates    325 k
 glib2                 x86_64    2.56.1-8.el7                  updates    2.5 M
 kpartx                x86_64    0.4.9-134.el7_9               updates     81 k
 libcurl               x86_64    7.29.0-59.el7_9.1             updates    223 k
 openssl-libs          x86_64    1:1.0.2k-21.el7_9             updates    1.2 M
 python                x86_64    2.7.5-90.el7                  updates     96 k
 python-libs           x86_64    2.7.5-90.el7                  updates    5.6 M
 systemd               x86_64    219-78.el7_9.2                updates    5.1 M
 systemd-libs          x86_64    219-78.el7_9.2                updates    418 k
 tzdata                noarch    2020f-1.el7                   updates    501 k
 vim-minimal           x86_64    2:7.4.629-8.el7_9             updates    443 k

Transaction Summary
================================================================================
Upgrade  16 Packages

Total download size: 20 M
Downloading packages:
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
warning: /var/cache/yum/x86_64/7/updates/packages/centos-release-7-9.2009.1.el7.centos.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
Public key for centos-release-7-9.2009.1.el7.centos.x86_64.rpm is not installed
--------------------------------------------------------------------------------
Total                                              3.7 MB/s |  20 MB  00:05     
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
Importing GPG key 0xF4A80EB5:
 Userid     : "CentOS-7 Key (CentOS 7 Official Signing Key) <security@centos.org>"
 Fingerprint: 6341 ab27 53d7 8a78 a7c2 7bb1 24c6 a8a7 f4a8 0eb5
 Package    : centos-release-7-9.2009.0.el7.centos.x86_64 (@CentOS)
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Updating   : systemd-libs-219-78.el7_9.2.x86_64                          1/32 
  Updating   : 1:openssl-libs-1.0.2k-21.el7_9.x86_64                       2/32 
  Updating   : coreutils-8.22-24.el7_9.2.x86_64                            3/32 
  Updating   : libcurl-7.29.0-59.el7_9.1.x86_64                            4/32 
  Updating   : python-libs-2.7.5-90.el7.x86_64                             5/32 
  Updating   : centos-release-7-9.2009.1.el7.centos.x86_64                 6/32 
  Updating   : systemd-219-78.el7_9.2.x86_64                               7/32 
Failed to get D-Bus connection: Operation not permitted
  Updating   : 7:device-mapper-libs-1.02.170-6.el7_9.3.x86_64              8/32 
  Updating   : 7:device-mapper-1.02.170-6.el7_9.3.x86_64                   9/32 
  Updating   : kpartx-0.4.9-134.el7_9.x86_64                              10/32 
  Updating   : python-2.7.5-90.el7.x86_64                                 11/32 
  Updating   : curl-7.29.0-59.el7_9.1.x86_64                              12/32 
  Updating   : glib2-2.56.1-8.el7.x86_64                                  13/32 
  Updating   : 2:vim-minimal-7.4.629-8.el7_9.x86_64                       14/32 
  Updating   : tzdata-2020f-1.el7.noarch                                  15/32 
  Updating   : 32:bind-license-9.11.4-26.P2.el7_9.3.noarch                16/32 
  Cleanup    : curl-7.29.0-59.el7.x86_64                                  17/32 
  Cleanup    : kpartx-0.4.9-133.el7.x86_64                                18/32 
  Cleanup    : 7:device-mapper-1.02.170-6.el7.x86_64                      19/32 
  Cleanup    : 7:device-mapper-libs-1.02.170-6.el7.x86_64                 20/32 
  Cleanup    : systemd-219-78.el7.x86_64                                  21/32 
  Cleanup    : python-2.7.5-89.el7.x86_64                                 22/32 
  Cleanup    : centos-release-7-9.2009.0.el7.centos.x86_64                23/32 
  Cleanup    : tzdata-2020d-2.el7.noarch                                  24/32 
  Cleanup    : 32:bind-license-9.11.4-26.P2.el7.noarch                    25/32 
  Cleanup    : python-libs-2.7.5-89.el7.x86_64                            26/32 
  Cleanup    : coreutils-8.22-24.el7.x86_64                               27/32 
  Cleanup    : 1:openssl-libs-1.0.2k-19.el7.x86_64                        28/32 
  Cleanup    : libcurl-7.29.0-59.el7.x86_64                               29/32 
  Cleanup    : systemd-libs-219-78.el7.x86_64                             30/32 
  Cleanup    : glib2-2.56.1-7.el7.x86_64                                  31/32 
  Cleanup    : 2:vim-minimal-7.4.629-7.el7.x86_64                         32/32 
  Verifying  : python-2.7.5-90.el7.x86_64                                  1/32 
  Verifying  : kpartx-0.4.9-134.el7_9.x86_64                               2/32 
  Verifying  : centos-release-7-9.2009.1.el7.centos.x86_64                 3/32 
  Verifying  : 7:device-mapper-1.02.170-6.el7_9.3.x86_64                   4/32 
  Verifying  : 32:bind-license-9.11.4-26.P2.el7_9.3.noarch                 5/32 
  Verifying  : tzdata-2020f-1.el7.noarch                                   6/32 
  Verifying  : coreutils-8.22-24.el7_9.2.x86_64                            7/32 
  Verifying  : libcurl-7.29.0-59.el7_9.1.x86_64                            8/32 
  Verifying  : 1:openssl-libs-1.0.2k-21.el7_9.x86_64                       9/32 
  Verifying  : curl-7.29.0-59.el7_9.1.x86_64                              10/32 
  Verifying  : python-libs-2.7.5-90.el7.x86_64                            11/32 
  Verifying  : 2:vim-minimal-7.4.629-8.el7_9.x86_64                       12/32 
  Verifying  : 7:device-mapper-libs-1.02.170-6.el7_9.3.x86_64             13/32 
  Verifying  : systemd-libs-219-78.el7_9.2.x86_64                         14/32 
  Verifying  : systemd-219-78.el7_9.2.x86_64                              15/32 
  Verifying  : glib2-2.56.1-8.el7.x86_64                                  16/32 
  Verifying  : 2:vim-minimal-7.4.629-7.el7.x86_64                         17/32 
  Verifying  : systemd-libs-219-78.el7.x86_64                             18/32 
  Verifying  : glib2-2.56.1-7.el7.x86_64                                  19/32 
  Verifying  : python-2.7.5-89.el7.x86_64                                 20/32 
  Verifying  : kpartx-0.4.9-133.el7.x86_64                                21/32 
  Verifying  : 32:bind-license-9.11.4-26.P2.el7.noarch                    22/32 
  Verifying  : centos-release-7-9.2009.0.el7.centos.x86_64                23/32 
  Verifying  : python-libs-2.7.5-89.el7.x86_64                            24/32 
  Verifying  : 7:device-mapper-1.02.170-6.el7.x86_64                      25/32 
  Verifying  : 7:device-mapper-libs-1.02.170-6.el7.x86_64                 26/32 
  Verifying  : systemd-219-78.el7.x86_64                                  27/32 
  Verifying  : coreutils-8.22-24.el7.x86_64                               28/32 
  Verifying  : 1:openssl-libs-1.0.2k-19.el7.x86_64                        29/32 
  Verifying  : libcurl-7.29.0-59.el7.x86_64                               30/32 
  Verifying  : curl-7.29.0-59.el7.x86_64                                  31/32 
  Verifying  : tzdata-2020d-2.el7.noarch                                  32/32 

Updated:
  bind-license.noarch 32:9.11.4-26.P2.el7_9.3                                   
  centos-release.x86_64 0:7-9.2009.1.el7.centos                                 
  coreutils.x86_64 0:8.22-24.el7_9.2                                            
  curl.x86_64 0:7.29.0-59.el7_9.1                                               
  device-mapper.x86_64 7:1.02.170-6.el7_9.3                                     
  device-mapper-libs.x86_64 7:1.02.170-6.el7_9.3                                
  glib2.x86_64 0:2.56.1-8.el7                                                   
  kpartx.x86_64 0:0.4.9-134.el7_9                                               
  libcurl.x86_64 0:7.29.0-59.el7_9.1                                            
  openssl-libs.x86_64 1:1.0.2k-21.el7_9                                         
  python.x86_64 0:2.7.5-90.el7                                                  
  python-libs.x86_64 0:2.7.5-90.el7                                             
  systemd.x86_64 0:219-78.el7_9.2                                               
  systemd-libs.x86_64 0:219-78.el7_9.2                                          
  tzdata.noarch 0:2020f-1.el7                                                   
  vim-minimal.x86_64 2:7.4.629-8.el7_9                                          

Complete!
Removing intermediate container dae8fb127c06
 ---> 47222e0a481d
Step 3/4 : RUN yum -y install python3
 ---> Running in f7d3e046bfc3
Loaded plugins: fastestmirror, ovl
Loading mirror speeds from cached hostfile
 * base: d36uatko69830t.cloudfront.net
 * extras: d36uatko69830t.cloudfront.net
 * updates: d36uatko69830t.cloudfront.net
Resolving Dependencies
--> Running transaction check
---> Package python3.x86_64 0:3.6.8-18.el7 will be installed
--> Processing Dependency: python3-libs(x86-64) = 3.6.8-18.el7 for package: python3-3.6.8-18.el7.x86_64
--> Processing Dependency: python3-setuptools for package: python3-3.6.8-18.el7.x86_64
--> Processing Dependency: python3-pip for package: python3-3.6.8-18.el7.x86_64
--> Processing Dependency: libpython3.6m.so.1.0()(64bit) for package: python3-3.6.8-18.el7.x86_64
--> Running transaction check
---> Package python3-libs.x86_64 0:3.6.8-18.el7 will be installed
--> Processing Dependency: libtirpc.so.1()(64bit) for package: python3-libs-3.6.8-18.el7.x86_64
---> Package python3-pip.noarch 0:9.0.3-8.el7 will be installed
---> Package python3-setuptools.noarch 0:39.2.0-10.el7 will be installed
--> Running transaction check
---> Package libtirpc.x86_64 0:0.2.4-0.16.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================
 Package                  Arch         Version              Repository     Size
================================================================================
Installing:
 python3                  x86_64       3.6.8-18.el7         updates        70 k
Installing for dependencies:
 libtirpc                 x86_64       0.2.4-0.16.el7       base           89 k
 python3-libs             x86_64       3.6.8-18.el7         updates       6.9 M
 python3-pip              noarch       9.0.3-8.el7          base          1.6 M
 python3-setuptools       noarch       39.2.0-10.el7        base          629 k

Transaction Summary
================================================================================
Install  1 Package (+4 Dependent packages)

Total download size: 9.3 M
Installed size: 48 M
Downloading packages:
--------------------------------------------------------------------------------
Total                                               13 MB/s | 9.3 MB  00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : libtirpc-0.2.4-0.16.el7.x86_64                               1/5 
  Installing : python3-setuptools-39.2.0-10.el7.noarch                      2/5 
  Installing : python3-pip-9.0.3-8.el7.noarch                               3/5 
  Installing : python3-3.6.8-18.el7.x86_64                                  4/5 
  Installing : python3-libs-3.6.8-18.el7.x86_64                             5/5 
  Verifying  : libtirpc-0.2.4-0.16.el7.x86_64                               1/5 
  Verifying  : python3-setuptools-39.2.0-10.el7.noarch                      2/5 
  Verifying  : python3-libs-3.6.8-18.el7.x86_64                             3/5 
  Verifying  : python3-3.6.8-18.el7.x86_64                                  4/5 
  Verifying  : python3-pip-9.0.3-8.el7.noarch                               5/5 

Installed:
  python3.x86_64 0:3.6.8-18.el7                                                 

Dependency Installed:
  libtirpc.x86_64 0:0.2.4-0.16.el7   python3-libs.x86_64 0:3.6.8-18.el7         
  python3-pip.noarch 0:9.0.3-8.el7   python3-setuptools.noarch 0:39.2.0-10.el7  

Complete!
Removing intermediate container f7d3e046bfc3
 ---> c6e84a7bdc8d
Step 4/4 : LABEL maintainer="willem@vanheemstrasystems.com"
 ---> Running in 5be03012c3ac
Removing intermediate container 5be03012c3ac
 ---> cd240ae8c10c
Successfully built cd240ae8c10c
```

Check the status of the docker images.

```
$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED              SIZE
<none>        <none>    cd240ae8c10c   About a minute ago   444MB
centos        centos7   8652b9f0cb4c   2 months ago         204MB
hello-world   latest    bf756fb1ae65   12 months ago        13.3kB
```

*** WE ARE HERE ***
