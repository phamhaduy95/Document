
command cheatsheet
`docker run -d --name image_name`
- `-d`: detached mode which make container run in background
- `--interactive`: tell docker to keep input stream stdin open for the container.
- `--tty`: request docker to create virtual terminal for the container, which permit passing signal
	- 
- 

suppose you use shell to start a container in foreground. To detach it while you are still in the shell, press `ctrl + P` and `ctrl + Q`



port mapping

giving name and label 

the docker diff subcommand show which files are affected since the image. 



The docker run subcommand starts up the container. The -p flag maps the  
container’s port 8000 to the port 8000 on the host machine, so you should now be  
able to navigate with your browser to http://localhost:8000 to view the application.  
The --name flag gives the container a unique name you can refer to later for  
convenience

running docker container as daemons add -d option
docker by default will run its container in the foreground


what happens when container si failed/terminated/paused
auto restart 
	on-failure\[:max-retry\]

Finally, the on-failure policy restarts only when the container returns a non-zero  
exit code (which normally means failing) from its main process


how docker isolate its containers 
utilize the components such as namespace, cgroup

bind mount 

diagnose the problem 


