# Jenkins-pipeline-for-multi-service-containerized-application with docker swarm.
Achieving the high availability by using docker swarm for hosting a multi service containerized application with automating the process by JENKINS CI/CD pipeline.

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


Configure:

1. Set hostnames 
  Access each machine and set hostname for each.

  For ( Manager ) Jenkins + Docker swarm machine:
    > sudo -s
    > hostnamectl set-hostname Jen-Doc-Man		#setting hostnamae as Jen-Doc-Man
    > sudo -i

  For Node machines
    > sudo -s
    > hostnamectl set-hostname Node1			# for node2, change the name
    > sudo -i

2. Install docker on machines
  Use multi-session mode:
    #Install docker 
    > yum install docker -y

    #start docker service
    > systemctl start docker.service

  exit multisession mode.

3. Initiate Docker swarm 
  In manager machine (Jen-Doc-Man):
  > docker swarm init
  
  Use the swarm join-token to join other machines

  check the nodes ( on manager machine)
  > docker node ls

4. Install Jenkins on Jen-Doc-Man machine
  vi Jenkins.sh
  add the script
  
  #run the script
  > sh Jenkins.sh

  --> if doesn't work see if Jenkins containers is already there as we did docker swarm -- for that docker service rm Jenkins

  Login to Jenkins and install recommended plugins.
  
5. Execution permission and restart the service
    On Jen-Doc-Man machine

  > chmod 777 /var/run/docker.sock
  > systemctl daemon-reload
  > systemctl restart docker.service

	
Pipeline:
	
Create new pipeline, Name = Swarm-based-application, Type= Pipeline

We go with parameterized for working/building the images with different names rather than writing same code multiple times.
Pipeline --> This project is parameterized --> Add parameters --> choice parameter

Name: image
Choices:
  internetbanking:v1
  mobilebanking:v1
  loans:v1
  insurance:v1

Add another parameter,
Name: repo
Choice:
  paramrepo/ib-img  					# paramrepo = repo name, ib-img = imagename-tag
  paramrepo/mb-img
  paramrepo/loans-img
  paramrepo/insurance-img

We need to login to the docker hub for pushing and pulling the images.
So for that we need username and password.
Rather than including the password in the pipeline, we can use env variables.

Jenkins --> Dashboard --> System ---> ENV --> Add ---> name = passeord, key=actual_password.

This ENV we created can be referred in pipeline using the $ sign.

Pipeline flow:
  Get repo from git ----> create an image for service ----> tag the image -----> Login and push to the docker hub  ---> pull the image and run containers using swarm ( using compose for same) 
  
  We are using docker-compose file as we are running multiple containers at a time.
```
pipeline{
  agent any
  stages{
    stage('checkout'){
      steps{ 
        git 'https://github.com/ReyazShaik/dockerproject.git'
        }
    }
    stage('Build'){
      steps{
        sh 'docker build -t $image .'
        }
    }
    stage('Tagging'){
      steps{
        sh 'docker tag $image $repo'
        }
    }
    stage('Push-image'){
      steps{ 
        sh 'docker login -u paramare -p $password'
        sh 'docker push $repo' 
        }
    }
    stage('Deploy'){
      steps{
        sh 'docker stack deploy -c docker-compose.yml Bank'
        }
    }

```

  Save -----> Build with parameter.

  At each run, we need to select 2 parameters ( image and repo )
  



Regarding Docker-compose file:
----------------------------------------------
  We have 4 services from the Bank application.
  so we pushed 4 images ( one for each build) to the hub and pulled the same during pull and creation.
  
  for each service, 
    image = repo/image_name:tag  ----> that saved as in the docker hub.
    specifying port == suing different port ofr each service.
    deploy == specifying the no. of replicas.
    Volumes == (optional) to specify volume to service.


Observations/Monitor :
		
1. we have 3 active machines ( ec2 instances / servers ) on docker swarm.
  > docker node ls
2. we have 4 services running on each of these machines
  > docker service ls
3. We have 3 replicas for each services
  > docker service ps internetbanking
4. We can access each service with ip of each machine and with respective ip
  http://ip_of_machine:81    
  http://ip_of_machine:82  
  http://ip_of_machine:83  
  http://ip_of_machine:84  
