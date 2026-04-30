Docker’s core functionality is all about isolation. Previous chapters have shown some of the benefits of process and filesystem isolation, and in this chapter you’ve seen network isolation.  
You could think of there being two aspects to network isolation:  
- Individual sandbox—Each container has its own IP address and set of ports to listen on without stepping on the toes of other containers (or the host).  
-  Group sandbox—This is a logical extension of the individual sandbox—all of the isolated containers are grouped together in a private network, allowing you to play around without interfering with the network your machine lives on (and incurring the wrath of your company network administrator!).

``` bash
docker network create \  
--driver bridge \  
--label project=dockerinaction \  
--label chapter=5 \  
--attachable \  
--scope local \  
--subnet 10.0.42.0/24 \  
--ip-range 10.0.42.128/25 \  
user-network
```

``` shell
ip -f inet -4 -o addr
```
nmap -sn 10.0.42.* -sn 10.0.43.* -oG /dev/stdout | grep Status


On occasion, it may be important to share other devices between a host and a  
specific container. Say you’re running computer vision software that requires access  
to a webcam, for example. In that case, you’ll need to grant access to the container  
running your software to the webcam device attached to the system; you can use the  
--device flag to specify a set of devices to mount into the new container.

Docker supports isolating the USR namespace. By default, user and group IDs  
inside a container are equivalent to the same IDs on the host machine. When  
the user namespace is enabled, user and group IDs in the container are  
remapped to IDs that do not exist on the host.


The best approach is to isolate the risk of running the program. First, make sure  
the application is running as a user with limited permissions

High-level system services are a bit different from applications. They’re not part of the  
operating system, but your computer makes sure they’re started and kept running.  
These tools typically sit alongside applications outside the operating system, but they  
often require privileged access to the operating system to operate correctly. They provide important functionality to users and other software on a system. Examples  
include cron, syslogd, dnsmasq, sshd, and docker.