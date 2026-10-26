# Automated CI/CD Pipeline Using Jenkins, Docker, AWS EC2, and Trivy

## 📌 Project Overview

This project demonstrates how to build, test, scan, and deploy a web application using an automated CI/CD pipeline with Jenkins, Docker, AWS EC2, GitHub, and Trivy.

The pipeline automates the application deployment process and helps improve deployment consistency, efficiency, and security.

## 🎯 Objectives

- Automate the application build and deployment process.
- Integrate GitHub with Jenkins for source code management.
- Build Docker images using a Dockerfile.
- Test the application before deployment.
- Scan Docker images for security vulnerabilities using Trivy.
- Deploy the application on an AWS EC2 instance using Docker.

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| AWS EC2 | Hosts Jenkins and the deployed application |
| Jenkins | Automates the CI/CD pipeline |
| Docker | Builds and runs application containers |
| Git | Version control |
| GitHub | Source code repository |
| Trivy | Docker image vulnerability scanning |
| Nginx | Serves the HTML web application |
| Linux / Ubuntu | Server operating system |

## 🏗️ Architecture

```text
Developer
   |
   v
GitHub Repository
   |
   v
Jenkins Pipeline
   |
   +--> 1. Checkout Source Code
   |
   +--> 2. Build Docker Image
   |
   +--> 3. Test Application
   |
   +--> 4. Scan Image Using Trivy
   |
   +--> 5. Deploy Docker Container
                 |
                 v
             AWS EC2
                 |
                 v
          Nginx Web Server
                 |
                 v
          Application in Browser
```

## ⚙️ CI/CD Pipeline Stages

### 1. Checkout

Jenkins retrieves the project source code from the GitHub repository.

### 2. Build Docker Image

Jenkins uses the Dockerfile to build a Docker image containing the Nginx web server and the application HTML page.

### 3. Test Application

The pipeline verifies that the HTML file exists, checks the expected HTML content, and confirms that the Docker image was created.

### 4. Security Scan Using Trivy

Trivy scans the Docker image for HIGH and CRITICAL security vulnerabilities.

In the successful scan performed for this project, zero vulnerabilities were reported at these severity levels.

### 5. Deploy Container

Jenkins removes the previous application container if present and starts a new container using the built Docker image.

The application container runs Nginx and is exposed through port `8081` on the EC2 host, mapped to port `80` inside the container.

## 📂 Project Structure

```text
jenkins-docker-cicd/
├── Dockerfile
├── Jenkinsfile
├── index.html
└── README.md
```

- **Dockerfile:** Defines the instructions for building the Docker image.
- **Jenkinsfile:** Contains the pipeline stages for checkout, build, test, security scanning, and deployment.
- **index.html:** Contains the web page displayed by the Nginx server.
- **README.md:** Documents the project, architecture, tools, and setup.

## 🐳 Docker Configuration

The Dockerfile uses Nginx Alpine as the base image.

```dockerfile
FROM nginx:alpine
RUN apk upgrade --no-cache
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

## 🚀 Deployment

The application is deployed as a Docker container on AWS EC2.

The container is started with the following command:

```bash
docker run -d \
  --name jenkins-demo \
  --restart unless-stopped \
  -p 8081:80 \
  jenkins-docker-cicd:latest
```

Check the running container:

```bash
docker ps
```

Test the application locally on the EC2 server:

```bash
curl -I http://localhost:8081
```

## 🌐 Application Access

After successful deployment, access the application using:

**Application URL:** `http://<EC2-PUBLIC-IP>:8081`

Replace `<EC2-PUBLIC-IP>` with the public IP address of your EC2 instance.

Ensure the EC2 security group permits inbound TCP traffic on port `8081` from your intended client IP addresses.

## ✅ Project Results

- Successfully configured and executed a Jenkins CI/CD pipeline.
- Built a Docker image for the web application.
- Validated the application and Docker image.
- Integrated Trivy vulnerability scanning.
- Successfully deployed the application in a Docker container on AWS EC2.
- Verified that all five Jenkins pipeline stages completed successfully.

## 🔐 Security Considerations

- Use restricted security group rules instead of exposing every port publicly.
- Scan container images regularly for known vulnerabilities.
- Keep the base image and its packages updated.
- Avoid storing credentials directly in source code.
- Configure Jenkins access securely and use HTTPS where appropriate.

**Note:** A successful Trivy scan does not guarantee that the image is completely free of security issues. This project checks for HIGH and CRITICAL findings.

## 📚 What I Learned

- CI/CD pipeline implementation using Jenkins.
- GitHub integration with Jenkins.
- Docker image creation and container deployment.
- Basic application validation in a CI/CD pipeline.
- Container image vulnerability scanning using Trivy.
- Deploying and accessing web applications on AWS EC2.
- Troubleshooting Linux disk space and server resource issues.

## 🔮 Future Improvements

- Configure a GitHub webhook to trigger Jenkins automatically on every push.
- Add stronger application tests and health checks.
- Publish Docker images to Docker Hub or Amazon ECR.
- Add notifications for pipeline success or failure.
- Implement monitoring using Prometheus and Grafana.
- Improve image versioning and vulnerability remediation.

