# &nbsp;		Docker Cheat Sheet

1. ##### To run Docker image 

###### &nbsp;	

###### &nbsp;	docker run <image name>



#### 2\. To check images 



###### &nbsp;	docker images



#### 3\. To check docker version



######  	docker -v



#### 4\. To pull docker images



######  	docker pull  <images name>



#### 5\. To search docker images



######  	docker search  <images name>

#### 

#### 6\. To check all  docker container



######  	docker ps -a  

###### &nbsp;	docker ps 



##### 7\. To pull docker images



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



##### 13\. To login docker hub



######  	docker login



##### 14\. To save docker changes



######  	docker commit 

##### 

##### 15\. To push docker changes



######  	docker push



##### 16\. To copy docker details from local computer



######  	docker copy 



##### 17\. To check logs docker container



######  	docker logs  <Container Name>



