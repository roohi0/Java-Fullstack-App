# Java Fullstack Application

A full stack web application demonstrating how a modern Java backend and Node.js frontend can be containerized with Docker and automatically deployed through a Jenkins CI/CD pipeline.

---

## 📌 Table of Contents

- [About the Project](#about-the-project)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Accessing the Application](#accessing-the-application)
- [CI/CD Pipeline](#cicd-pipeline)
- [What This Project Demonstrates](#what-this-project-demonstrates)
- [Author](#author)

---

## About the Project

This project is a full stack web application built with **Spring Boot** on the backend and **Node.js/JavaScript** on the frontend. Both services are containerized using Docker and orchestrated with Docker Compose, enabling consistent and reproducible environments across local development and production.

The project is integrated with a **Jenkins CI/CD pipeline** that automatically pulls the latest code, rebuilds containers, and deploys the updated application on every pipeline trigger.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Java, Spring Boot, Maven |
| Frontend | Node.js, JavaScript |
| Containerization | Docker, Docker Compose |
| CI/CD | Jenkins |
| Version Control | Git, GitHub |

---

## Project Structure

```
Java-Fullstack-App/
│
├── backend/
│   ├── src/                    # Java source files
│   └── pom.xml                 # Maven dependencies and build config
│
├── frontend/
│   ├── src/                    # JavaScript source files
│   └── package.json            # Node.js dependencies
│
├── docker-compose.yml          # Multi-container orchestration
├── Jenkinsfile                 # CI/CD pipeline definition
└── README.md
```

---

## Prerequisites

Make sure the following are installed before running the application locally:

- [Docker](https://docs.docker.com/get-docker/) (v20+)
- [Docker Compose](https://docs.docker.com/compose/install/) (v2+)
- [Git](https://git-scm.com/)

> For the Jenkins pipeline, you will also need a running Jenkins instance with Docker access.

---

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/roohi0/Java-Fullstack-App.git
cd Java-Fullstack-App
```

### 2. Start the Application

```bash
docker-compose up -d
```

This will build and start both the frontend and backend containers in detached mode.

### 3. Stop the Application

```bash
docker-compose down
```

---

## Accessing the Application

Once the containers are running, open your browser and navigate to:

| Service | URL |
|---|---|
| Frontend | http://localhost:3000 |
| Backend API | http://localhost:8080 |

> **Note:** Confirm the ports in `docker-compose.yml` if these differ in your setup.

---

## CI/CD Pipeline

This project includes a **Jenkinsfile** that automates the full deployment lifecycle.

### Pipeline Stages

| Stage | Description |
|---|---|
| `checkout` | Pulls the latest code from GitHub |
| `stop existing` | Shuts down any currently running containers |
| `build` | Rebuilds images and starts updated containers |
| `clean up` | Removes unused Docker images and containers |

### Pipeline Script

```groovy
pipeline {
    agent any

    stages {

        stage('checkout') {
            steps {
                git 'https://github.com/roohi0/Java-Fullstack-App.git'
            }
        }

        stage('stop existing') {
            steps {
                sh 'docker-compose down'
            }
        }

        stage('build') {
            steps {
                sh 'docker-compose up -d --build'
            }
        }

        stage('clean up') {
            steps {
                sh 'docker image prune -f'
            }
        }

    }
}
```

> ⚠️ **Note:** The original pipeline used `docker system prune -a`, which removes **all** unused images — including ones you may want to keep. The updated script above uses `docker image prune -f` instead, which only removes dangling (unreferenced) images and is safer for most setups.

---

## What This Project Demonstrates

- Full stack application architecture with separate frontend and backend services
- Containerizing applications using Docker with multi-stage builds
- Managing multi-service applications using Docker Compose
- Automating build and deployment with a Jenkins CI/CD pipeline
- Using GitHub for version control and source of truth

---

## Author

**Roohi Gooty**  
GitHub: [github.com/roohi0](https://github.com/roohi0)
