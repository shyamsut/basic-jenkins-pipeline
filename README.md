# 🔄 CI/CD Pipeline with Jenkins & Docker

A simple automated pipeline setup using Jenkins and Docker to build and deploy a Node.js application. This project demonstrates how to integrate continuous integration and continuous deployment in a real-world development environment.

---

## 🧰 Tools & Technologies

- [Jenkins](http://65.2.169.108:8080/) – Automation server
- [Docker](https://hub.docker.com/repositories/shyamsuthar8824) – Containerization
- [GitHub](https://github.com/shyamsut/basic-jenkins-pipeline/) – Source code versioning
- [Node.js](http://65.2.169.108:3000/) – Sample application runtime

---

## 📦 Project Structure

```bash
├── Jenkinsfile              # CI/CD pipeline definition
├── Dockerfile               # Docker image configuration
├── package.json             # Node.js project metadata
├── server.js                # Simple web server (example)
└── README.md                # Project documentation
🎯 Objective
The goal is to automate the process of building, testing, and deploying a web application using Jenkins pipeline integrated with Docker.

🚀 Pipeline Flow
Whenever new code is pushed to the GitHub repository:

Checkout – Jenkins pulls latest code from GitHub.

Build – Installs dependencies via npm install.

Test – Runs unit or functional tests via npm test.

Dockerize – Builds a Docker image from the source.

Deploy – Runs the Docker container exposing the app on a specified port.

📜 Jenkinsfile Overview


pipeline {
    agent any

    environment {
        IMAGE_NAME = 'simple-docker-app'
        CONTAINER_NAME = 'simple-docker-container'
        HOST_PORT = '3000'
    }

    stages {
        stage('📥 Checkout') {
            steps {
                checkout scm
            }
        }

        stage('🧪 Test') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }

        stage('🐳 Build Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('🧹 Clean Up') {
            steps {
                sh "docker rm -f ${CONTAINER_NAME} || true"
            }
        }

        stage('🚀 Deploy') {
            steps {
                sh "docker run -d -p ${HOST_PORT}:3000 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful! Visit: http://<your-server-ip>:3000"
        }
        failure {
            echo "❌ Deployment failed. Please check console logs."
        }
    }
}
