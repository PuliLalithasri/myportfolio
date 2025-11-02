pipeline {
    agent any

    environment {
        IMAGE_NAME = "lalitha-portfolio"
        CONTAINER_NAME = "portfolio-container"
        PORT = "8081"
    }

    options {
        timestamps()
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
                script {
                    echo "🐳 Building Docker image..."
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Stop Old Container') {
            steps {
                echo "🛑 Stopping and removing old container (if running)..."
                script {
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                }
            }
        }

        stage('Run New Container') {
            steps {
                script {
                    echo "🚀 Deploying new Docker container..."
                    sh "docker run -d -p ${PORT}:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "🔍 Checking container status..."
                sh "docker ps | grep ${CONTAINER_NAME}"
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful! Portfolio live at: http://localhost:${PORT}"
        }
        failure {
            echo "❌ Build or deployment failed. Please check Jenkins logs."
        }
    }
}
