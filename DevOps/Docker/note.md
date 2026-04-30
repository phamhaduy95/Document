Disposability  
Maximize robustness with fast startup and graceful shutdown.

As discussed in Chapter 7, Docker sends standard Unix signals to containers when it  
is stopping or killing them; therefore, any containerized application can detect these  
signals and take the appropriate steps to shut down gracefully.


controlling process inside container 
Unless you kill the top-level process in the container (PID 1 inside the container),  
killing a process will not terminate the container itself. That might be desirable if you  
were killing a runaway process, but it might leave the container in an unexpected  
state. Developers probably expect that all the processes are running if they can see  
their container in docker container ls. It could also confuse a scheduler like Mesos  
or Kubernetes or any other system that is health-checking your application. Keep in  
mind that containers are supposed like a single bundle to the outside world. If you  
need to kill off something inside the container, it’s best to replace the whole container.  
Containers offer an abstraction that tools interoperate with

docker 

difference between docker and VN
docker is application-oriented while VM is operation system oriented
docker containers are operated on one single shared kernel while VM has it own kernel managed by hypervisor 
docker containers are design to run on one principle process, not managing multiple processes at once.

The history of container use has tended toward using them to isolate workloads to  
“one service per container


distinction between `docker kill` and `docker stop`
you want to cleanly terminate a container 
The kill program works by sending a TERM (a.k.a. signal value 15) signal to the  
process specified, unless directed otherwise. This signal indicates to the program that  
it should terminate, but it doesn’t force the program

docker stop will  send the TERM signal at first and wait for 10 seconds and then send the KILL signal in case the service is not stopped.


