# Jenkins-pipeline-for-multi-service-containerized-application with docker swarm.
Achieving the high availability by using docker swarm for hosting a multi service containerized application with automating the process by JENKINS CI/CD pipeline.

   <img width="859" height="469" alt="image" src="https://github.com/user-attachments/assets/e4f924e0-7705-4f7b-8948-2e96c059ccf4" />




--------------------------------


```
		---------------			-------------------------	  	   -------------------
		|   GIT   |     -------->	|	JENKINS		|   <--------->	   |	Docker hub   |
		---------------			|  Docker swarm		|	           -------------------
						  -----------------------	   
							|
							|
				   ---------------------------------------------
				  |						|
				Node1				       	      Node2
```


Objective:
--------------
Creating a Jenkins pipeline for high available, multi-container application using docker swarm.

Outcomes:
  1. Writing pipeline that integrates docker swarm  
  2. Working with parameters from Jenkins.  
  3. Saving password (hidden) to include in pipeline.  
  4. Integrating Docker Hub for pushing and pulling images.   


Setup:  
	1 AL2 machine with t2.medium, and 10gb size   --> to handle Jenkins and docker swarm   
	2 AL2 machines with t2.micro 				--> for Node1 and Node2  


