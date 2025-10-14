# SpringBuild CI/CD Automation Pipeline

### Overview
SpringBuild CI Automation Pipeline is a Jenkins-driven Continuous Integration (CI/CD) project that automates the build, test,  and deploy of the Docker image packaging process for a Spring Framework Java application.  
It ensures every code commit is validated, built, containerized in a consistent and deployed to EC2 server running a docker container engine in a repeatable way — reducing manual errors and improving delivery speed.

---

### Key Features
- Automated Build:Uses Jenkins to compile and package Spring applications via Maven.   
- Containerization: Builds and tags Docker images for deployment readiness.
- deployment: Deploy the resulting conatiner image into a docker host on EC2 server

---

### Architecture
```
Developer → GitHub Repo → Jenkins CI Pipeline → Maven Build  → Docker Build → Push to Registry → Pull the image → Deploy on the EC2 server
```

---

### Tech Stack
- Jenkins – CI orchestration  
- Maven – Build automation  
- Docker – Image creation and distribution  
- Java / Spring Framework – Application framework  
- GitHub – Source code management
- AWS - Dploy the docker image

---

### Pipeline Stages
1. Checkout Code – Pulls the latest commit from GitHub.  
2. Build with Maven – Compiles and packages the Spring app.  
3. Build Docker Image – Packages the JAR into a Docker container.  
4. Push to Registry – Publishes the image to Docker Hub.
5. Deploy the application on AWS EC2 server running Docker. 

---

### Prerequisites
- Jenkins server with Docker installed and credentials.  
- Jenkins credentials for image registry (Docker Hub).
- AWS Ec2 server for deployment.
- jenkins credentials  with SSH agent  plugin for remote access to EC2 server for deployment.
- EC2 server with docker installed.
 

---

### Usage
1. Clone the repository.  
   ```bash
   git clone https://github.com/AbdelhamedBadawy47/simple-spring-app-ci-pipeline
   ```
2. Configure Jenkins credentials  (dockerhub-credentials + EC2 remote server credentials). 
3. Run the pipeline manually or via webhook trigger.  
4. Monitor the stages in Jenkins UI.
5. Checkout the changes in image version tags in dockerhub registery and the conatiners in EC2 server

---

### Future Enhancements
- Extend pipeline to include Continuous Deployment (CD) to Kuberentes cluster for larger application scale.   
