pipeline {
    agent any

    environment {
        IMAGE_NAME = "lalitha-portfolio"
        CONTAINER_NAME = "portfolio-container"
        PORT = "8080"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "📦 Cloning the repository..."
                git branch: 'main', url: 'https://github.com/PuliLalithasri/myportfolio.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Building Docker image..."
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Stop Old Container') {
            steps {
                echo "🛑 Stopping and removing old container if exists..."
                script {
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                }
            }
        }

        stage('Run New Container') {
            steps {
                echo "🚀 Running new Docker container..."
                script {
                    sh "docker run -d -p ${PORT}:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest"
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful! Portfolio is live on port ${PORT}"
        }
        failure {
            echo "❌ Build or deployment failed. Please check logs."
        }
    }
}
