#  		             Docker Cheat Sheet

1. ##### To run Docker image

######  

######  	docker run <image name>



#### 2\. To check images



######  	docker images



#### 3\. To check docker version



######  	docker -v



#### 4\. To pull docker images



######  	docker pull  <images name>



#### 5\. To search docker images



######  	docker search  <images name>

#### 

#### 6\. To check all  docker container



######  	docker ps -a

######  	docker ps



##### 7\. To pull docker images

######  	  docker pull <name of the image >



######  	docker run --name <name of the container> -d <name of the image>



##### 8\. To run docker container in interactive mode



######  	docker run --name <name of the container> -it -d <name of the image>





##### 7\. To go inside docker container



######  	docker exec -it <Container id> <command>



##### 8\. To check docker container info



######  	docker inspect <Container id>



##### 9\. To stop docker container



######  	docker stop <Container id> / <Container Name>





##### 10\. To remove docker container run history



######  	docker rm <Container id>



##### 11\. To remove docker images



######  	docker rmi <Image Name>



##### 12\. To restart docker container



######  	docker restart <Container id> / <Container Name>



##### 13\. To login/logout docker hub



######  	docker login

######  	docker logout



##### 14\. To save docker changes



######  	docker commit

##### 

##### 15\. To push docker changes



######  	docker push



##### 16\. To copy docker details from local computer



######  	docker copy



##### 17\. To check logs docker container



######  	docker logs  <Container Name>



##### 18\. To give storage docker container



######  	docker volume

######  	docker run -v absolute path /container path   <image name>

 

 	First create storage
docker volume create <volume name >
---

######  	docker run -v storage name path    <image name>

 

######  	To delete the volume

 

######  	   docker volume rm <volume name>

 



######  	  3 types of volume and there commands

######  

 		Named Volume

\---

######  		docker run -v <name of the volume >:<container storage path >

###### 

######  		Anonymous Volume

######  

######  		docker run -v Mount\_PATH

###### 

######  		Bind Mount

######  

 		docker run -v HOST\_PATH:CONTAINER\_PATH

\---

 	   To delete unallocated volume

\---

######  		docker volume prune

 





##### 19\. To build docker image



######  	docker build -t <image name> <path of the image>/.



##### 20\. To create/delete custom network in docker/list docker network



######  	docker network create <network-name>

######  	docker network ls

#####       docker network rm my-network

##### 21\. To execute commands in a running container (Troubleshoot Commands)



######  	docker exec -it <CONT\_ID> /bin/bash

######  	docker exec -it <CONT\_ID> /bin/sh

##### 

##### 22\. Commands for docker compose

######  	docker compose -f <file name .yaml> up -d

######  	docker compose -f <file name .yaml> down



##### 23\. To push docker image



######  	docker push  <image name>

##### 

##### 24\. Mount command in docker



######  	docker run --mount type=<type of the storage >, src=<volume name >, dst=<mount path>  <image name>



##### 25\. Tag command in docker



######  	docker tag <image name> <tag name>



######   docker exec myauthserviceapp env | grep DB\_

