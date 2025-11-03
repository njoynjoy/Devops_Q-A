DOCKER
```	Docker container exits immediately after you build docker image and run the container? ```
Docker containers are meant to run single process, if the command or process completes quickly (short lived) or the command fails where u entered something wrong in the entrypoint 
We can check that through docker log
 <img width="648" height="240" alt="image" src="https://github.com/user-attachments/assets/8ebbb953-a3d1-4103-b5b4-8af4e0f9b596" />

If we don’t want the container to be stooped we can use docker run -it 

```	What is the purpose of `expose` in Docker? ```
Docker File has keywords like "run, CMD, entry point". Similarly, we have a keyword called "expose".
For example, we have a Nginx web application running in port 80, we'll be keeping in the file as expose 80. But when running the container, we will be using a command like "docker run -p 80:80" so we are defining it in the container run . What is the use of "expose"?
In docker file expose keyword is used for documentation purpose When we ran this container one year ago, we passed it to that other team so they would understand that the application is running in the port 80 through expose 80 in file  through the Dockerfile, not going through the whole source code.
So expose does not publish or open port to the outside world. It is just used as a meta data 
We can use expose keyword when working with docker compose where u having front end and backend and u configured expose port 8080 then it means the front need to talk to backend on port 8080 and in db u metioned something expose 3304- it means backend need to interact with db on port 3304.in docker compose read the expose keyword in docker file

  ``` Port is not accessible in the localhost even after Port mapping in Docker? ```
After port mapping is done correctly, follow the troubleshooting steps.\
1.	Re check the port mapping is done correctly or not through the command `docker run -p port_number`
2.	Check whether the assigned port is free or not in the local machine. By command netstat or lsof-list of open files
3.	Check any firewalls running on the machine. If you're running in Ubuntu, you can check UFW. If the firewall is running, disable the firewall, and then it will run.
4.	I will understand which language the application was written in if it is in Python, And the developer is using Flask when running the application developer might have mentioned app.listen (80, ‘127.0.0.1’) when this is provided then it doesnot work on all the hosts only local host it means when we login to the container and run curl then only it is accessible and application will work because the port is mapped to the application in host so we will inform the developer to update the app.listen(80, ‘0.0.0.0’) after this we can access from the browser
5.	Docker log for further troubleshooting or docker inspect 

  ``` Data loss continues when it stop  and restarts. How will you fix? ```
This is related to docker volumes
By default docker containers are ephemeral in nature—short lived means what ever data is there it is lost when it stoped and restared
However, we can use the concept of Docker volumes to preserve the data in the container. Using the command 
docker volume create mydata 
so when running the container use the command below
docker run -v mydata:/app/data ‘name of the contaner’ --- mydata:/app/data is mapping the volume to the directory in local
When the container restarts, it will take the data from /app/data
We can also use bind mounts but volumes are more preferable

```	You made a change in the code, rebuilt the image, but the changes are not reflecting? ```
This happens because docker uses caching, docker uses layer caching  instructions with in the docker file is layered and cache is maintained by docker, in some cases docker might not recognize the change at a particular instruction file and because of that it will not rebuild that part but take from the caching, this occurs very very rare to avoid that use the command
docker build –-no-cache -t <’image name’> location of docker file
<img width="479" height="278" alt="image" src="https://github.com/user-attachments/assets/3caf974f-60c9-41df-a0c2-8128a2316f25" />
 

```	APP crashes with ‘permission denied’ in container but works fine on localhost? ```
When running in the local host the user may have all the elevated access, but when running in the virtual machine the user might not have the access to the file where the docker file is located command to fix is
Chown appuser:appuser  /location of the file
 or chmod +x executable permissions ‘file ocatioin’

```	Docker host is running out of disk space. How do you clean it? ```
To check the disk space
 docker system df – gives current disk space
docker system prune -a which will helps to clean up the dangling images or may be we have created the docker images but not tagged those are less significant, suppose 6 months back you have ran a docker image but not used such images are removed by docker system prune -a 
docker volume prune – it will remove unused volumes 
and after can run docker system df to check the utilization

```	How to debug a live container? ```
docker exec -it ‘container_name’ /bin/bash
or
docker exec -it ‘container name’ sh
```	Which container registry do you use in your organization? ```
we are on aws so we will be having the ECR – elastic container registry
quay.io,ghcr.io, where quay.io is redhat container registry which has no rate limit
ghcr.io, github container registry.io

``` 	Explain the difference between CMD and ENTRYOINT? ```
Both CMD and Entrypoint server same service, when you run the container these are the first commands to get executed.
CMD – the default argument that passed in docker file can be overridden
Entry point: the default argument will be append with the argument that passed docker file.

Can you overwrite entry point ??—yes when running the command if we pass 
docker run – -entry point
 we can override

```	What docker commands you use day to day basis? ```
Docker build-build an image from docker file
Docker run – once the image is built to run the container I use docker run
Docker ps-  to list what are the running containers on the host
Docker images- list of images on the host
Docker logs- to check the logs of a running container
Docker exec
Docker system prune

```	When will you forcefully remove a container and why? ```
1.	When a container is struck or container is unresponsive
2.	When container is forcefully restarted.
3.	Ci/cd pipeline when container is created we forcefully remove it because if pipelines run gain container will create again
docker -rm -f <container id>
