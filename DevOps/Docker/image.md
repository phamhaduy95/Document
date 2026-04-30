image layer are additive 

layering 
copy-on-write mechanism to reduce amount of disk space usage
everything is stored as image.
image is served as base data, one new data generated in each container will be allocated with new space. Thus, makes memory usage in docker is efficient.  
allow us to initiate a large amount of containers without running out all disk usage.
A Docker image is the template for a running container. This is similar to the  
difference between a program executable and a running process

prototype software
packaging software

you want to guarantee the container built from a specific and unchanged image.


Being more specific makes the result of your action more predictable and debuggable, as there’s less ambiguity about which Docker image is or was downloaded

Building image
Each Dockerfile command creates a single new layer on top of the previous one, but using && in your RUN statements effectively ensures that several  
commands get run as one command. This is useful because it can keep your  
images small. If you run a package update command like apt-get update  
with an install command in this way, you ensure that whenever the packages  
are installed, they’ll be from an updated package cache.

CMD or ENTRYPOINT

You want to define the command the container will run, but leave the command’s  
arguments up to the user**Flexible**. The `CMD` acts as a default command that can be easily overridden by specifying a new command at runtime.
A consequence of the design of `Dockerfile` and their production of Docker images is  
that the final image contains the data state at each step in the `Dockerfile`. In the  
course of building your images, secrets may need to be copied in to ensure the build  
can work.
These secrets may be SSH keys, certificates, or password files. Deleting these  
secrets before committing your image doesn’t provide you with any real protection, as  
they’ll be present in higher layers of the final image

tagging image 

Tags are both an important way to uniquely identify an image and a convenient way to  
create useful aliases. Whereas a tag can be applied to only a single image in a repository, a single image can have several tags. This allows repository owners to create useful versioning or feature tags

private image registries
Union filesystems use a pattern called copy-on-write, and that makes implementing  
memory-mapped files (the mmap system call) difficult. Some union filesystems provide  
Summary 61  
implementations that work under the right conditions, but it may be a better idea to  
avoid memory-mapping files from an image.

``` bash 
docker image history ubuntu-git:removed
```

You can flatten images by saving the image to a TAR file with docker image save, and  
then importing the contents of that filesystem back into Docker with docker image  
Exporting and importing flat filesystems 139  
import. But that’s a bad idea, because you lose the original image’s metadata, its  
change history, and any savings customers might get when they download images with  
the same lower levels. The smarter thing to do in this case is to create a branch.


A typical Dockerfile that follows this security practice includes the following steps:

1. Use the `groupadd` command to create a new group.
2. Use the `useradd` command to create a new user and add them to the newly created group.
3. Use the `chown` command to change the ownership of the application's working directory to the new user and group.
4. Use the `USER` instruction to switch the context of subsequent commands to the non-root user.