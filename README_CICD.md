# 🛒 Ekart – Spring Boot Shopping Cart Application

Ekart is a **Spring Boot–based Shopping Cart application** integrated with a **production-ready CI/CD pipeline** using **Jenkins, Docker, SonarQube, and Trivy**.

This project demonstrates **DevOps best practices** including automated builds, static code analysis, vulnerability scanning, containerization, and deployment.

---

## 🚀 Key Features

- RESTful APIs built with **Spring Boot**
- Automated **CI/CD pipeline using Jenkins**
- Code quality & security analysis with **SonarQube**
- File system & container image scanning using **Trivy**
- Dockerized application using **Java 17**
- Automated deployment using Docker containers
- Health check validation after deployment

---

## 🧰 Technologies & Tools

| Tool / Technology | Version / Notes |
|------------------|----------------|
| Java             | 17 (Eclipse Temurin) |
| Spring Boot      | Latest Stable |
| Maven            | 3.9.x |
| Jenkins          | 2.x |
| Docker           | Latest |
| SonarQube        | LTS (Community Edition) |
| Trivy            | Latest |
| Git & GitHub     | Version Control |

---

## 📂 Project Structure

Ekart/
├── docker/
│ └── Dockerfile
├── src/
│ └── main/java/... # Spring Boot source code
├── pom.xml # Maven project configuration
├── Jenkinsfile # Jenkins CI/CD pipeline
└── README.md

### Important Files
- **`docker/Dockerfile`** → Builds and runs the Spring Boot application
- **`Jenkinsfile`** → Complete CI/CD pipeline definition
- **`pom.xml`** → Project dependencies and build configuration

---

## 🔄 CI/CD Pipeline Overview

The Jenkins pipeline performs the following stages:

1. **Checkout**
   - Fetches the latest source code from GitHub

2. **Build & Test**
   - Builds the project using Maven
   - Executes unit tests

3. **SonarQube Analysis**
   - Performs static code analysis
   - Enforces code quality gates

4. **Trivy File System Scan**
   - Scans project dependencies for vulnerabilities

5. **Docker Build**
   - Builds Docker image using the Dockerfile

6. **Trivy Image Scan**
   - Scans Docker image for HIGH and CRITICAL vulnerabilities

7. **Push Docker Image**
   - Pushes versioned and `latest` images to Docker Hub

8. **Deploy Application**
   - Runs the application inside a Docker container

9. **Health Check**
   - Verifies application availability via HTTP endpoint

## 🔐 Jenkins Prerequisites

Ensure the Jenkins agent has:

- JDK 17 configured (`jdk17`)
- Maven 3.9 configured (`maven3.9`)
- Docker installed and running
- Trivy installed
- Sonar Scanner configured
- SonarQube server configured as `SonarQube-Server`
- Docker Hub credentials stored in Jenkins as:
  - **Credentials ID:** `docker-cred`
