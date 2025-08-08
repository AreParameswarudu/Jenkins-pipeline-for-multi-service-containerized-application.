# Jenkins-pipeline-for-multi-service-containerized-application with docker swarm.
Achieving the high availability by using docker swarm for hosting a multi service containerized application with automating the process by JENKINS CI/CD pipeline.

   <img width="859" height="469" alt="image" src="https://github.com/user-attachments/assets/e4f924e0-7705-4f7b-8948-2e96c059ccf4" />




--------------------------------
# Project Context:
In a cloud-native development setup, there was a need to automate the deployment of a containerized application that demanded high availability. The challenge was to ensure consistent, scalable, and secure delivery of services using Docker Swarm, while maintaining a clear DevOps workflow.

# 🕵️‍♂️ Objective:
Build a production-grade Jenkins pipeline capable of handling a multi-container application lifecycle — from build and image management to deployment across a Swarm cluster — **with credential security and environment flexibility**.

# 🏋️‍♂️ Implementation Highlights: 🏋️‍♂️
* Deployed infrastructure on AWS using three EC2 instances:

 	 1. t2.medium Amazon Linux 2 instance hosted Jenkins and served as the Swarm manager.

 	 1. t2.micro Amazon Linux 2 instances acted as Swarm worker nodes.

* Configured Jenkins with required Docker and Git integrations.

* Developed a parameterized pipeline to support dynamic input (e.g., image tags or environment variables).

* Integrated Docker Hub for image pushing/pulling, managing credentials securely using Jenkins’ credential management.

* Automated the deployment using docker stack deploy, ensuring the application runs in a resilient and replicated manner across nodes.

# 💻 Impact:
* Achieved fully automated CI/CD delivery, reducing manual deployment overhead.
  
* Secured pipeline with credential encryption and reusable parameters.

* Enabled repeatable deployments and streamlined updates across environments.

* Strengthened reliability through Docker Swarm’s high availability features, aligning with modern DevOps practices.

## Outcomes:
  1. Writing pipeline that integrates docker swarm  
  2. Working with parameters from Jenkins.  
  3. Saving password (hidden) to include in pipeline.  
  4. Integrating Docker Hub for pushing and pulling images.   



