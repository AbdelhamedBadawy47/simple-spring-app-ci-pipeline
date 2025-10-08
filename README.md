# SpringBuild CI Automation Pipeline

### Overview
SpringBuild CI Automation Pipeline is a Jenkins-driven Continuous Integration (CI) project that automates the build, test, and Docker image packaging process for a Spring Framework Java application.  
It ensures every code commit is validated, built, and containerized in a consistent, repeatable way — reducing manual errors and improving delivery speed.

---

### Key Features
- **Automated Build:** Uses Jenkins to compile and package Spring applications via Maven.  
- **Continuous Integration:** Automatically triggered on Git commits using GitHub webhooks.  
- **Containerization:** Builds and tags Docker images for deployment readiness.  
- **Test Automation:** Runs JUnit test suites to ensure code quality before packaging.  
- **Scalable Design:** Easily extendable to include Continuous Deployment (CD) stages.

---

### Architecture
```
Developer → GitHub Repo → Jenkins CI Pipeline → Maven Build  → Docker Build → Push to Registry
```

---

### Tech Stack
- **Jenkins** – CI orchestration  
- **Maven** – Build automation  
- **Docker** – Image creation and distribution  
- **Java / Spring Framework** – Application framework  
- **GitHub** – Source code management  

---

### Pipeline Stages
1. **Checkout Code** – Pulls the latest commit from GitHub.  
2. **Build with Maven** – Compiles and packages the Spring app.  
3. **Build Docker Image** – Packages the JAR into a Docker container.  
4. **Push to Registry** – Publishes the image to Docker Hub (or Azure ACR).  

---

### Prerequisites
- Jenkins server with Docker installed.  
- Jenkins credentials for image registry (Docker Hub or ACR).  
- Configured GitHub webhook to trigger pipeline runs.  

---

### Usage
1. Clone the repository.  
   ```bash
   git clone https://github.com/AbdelhamedBadawy47/simple-spring-app-ci-pipeline
   ```
2. Configure Jenkins credentials (`dockerhub-credentials`).  
3. Run the pipeline manually or via webhook trigger.  
4. Monitor the build stages in Jenkins UI.  

---

### Future Enhancements
- Extend pipeline to include Continuous Deployment (CD).   
