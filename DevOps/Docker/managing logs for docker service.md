docker logs display any log message that is written via stdout 
the problem is that, the log message is not truncated and kept very long, which make reading a log hard. 

the docker inspect command will display all metadata that Docker maintains for a container

`docker diff` show change in its filesystem

to restore a service as fast as possible

PID1 and init system

An init system is a program that’s used to launch and maintain the state of other programs. Any process with PID 1 is treated like an init process by the Linux kernel (even if it is not technically an init system). In addition to other critical functions, an init system starts other processes, restarts them in the event that they fail, transforms and forwards signals sent by the operating system, and prevents resource leaks. It is common practice to use real init systems inside containers when that container will run multiple processes or if the program being run uses child processes

By default, every Docker container has its own PID namespace, isolating process  
information for each container.  
 Docker identifies every container by its generated container ID, abbreviated  
container ID, or its human-friendly name.