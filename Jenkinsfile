pipeline {
    agent any

    options {
        timestamps() // 🕒 Show timestamps in console output
        timeout(time: 10, unit: 'MINUTES') // ⏰ Limit build time
        skipStagesAfterUnstable()
    }

    environment {
        IMAGE_NAME = 'jenkins-docker-pipeline-app'
        CONTAINER_NAME = 'jenkins-app-container'
        HOST_PORT = '3000'
        CONTAINER_PORT = '3000'
    }

    stages {
        stage('📥 Checkout Code') {
            steps {
                echo 'Cloning source code from GitHub...'
                checkout scm
            }
        }

        stage('🐳 Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image: ${IMAGE_NAME}"
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('🧹 Remove Old Container') {
            steps {
                script {
                    echo "Stopping and removing old container if exists..."
                    sh "docker rm -f ${CONTAINER_NAME} || true"
                }
            }
        }

        stage('🚀 Run Docker Container') {
            steps {
                script {
                    echo "Starting container: ${CONTAINER_NAME}"
                    sh "docker run -d -p ${HOST_PORT}:${CONTAINER_PORT} --name ${CONTAINER_NAME} ${IMAGE_NAME}"
                }
            }
        }

        stage('📊 Show Running Containers') {
            steps {
                echo "Listing all running containers..."
                sh "docker ps"
            }
        }
    }

    post {
        success {
            echo '✅ Deployment completed successfully! Visit http://<server-ip>:3000'
        }
        failure {
            echo '❌ Something went wrong during the deployment.'
        }
        always {
            echo '📌 Pipeline execution ended. Check Blue Ocean or console output for details.'
        }
    }
}

