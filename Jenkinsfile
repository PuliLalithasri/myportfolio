pipeline {
    agent any

    environment {
        IMAGE_NAME = "lalitha-portfolio"
        CONTAINER_NAME = "portfolio-container"
        PORT = "8081"      // Changed to avoid Jenkins port conflict
    }

    options {
        timestamps()       // Adds timestamps to Jenkins console
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
                    // Enable colorized logs for this stage
                    ansiColor('xterm') {
                        sh "docker build -t ${IMAGE_NAME}:latest ."
                    }
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
                    ansiColor('xterm') {
                        sh "docker run -d -p ${PORT}:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest"
                    }
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "🔍 Checking if the container is running..."
                sh "docker ps | grep ${CONTAINER_NAME}"
            }
        }
    }

    post {
        success {
            echo "\033[1;32m✅ Deployment successful! Portfolio is live at: http://localhost:${PORT}\033[0m"
        }
        failure {
            echo "\033[1;31m❌ Build or deployment failed. Please check Jenkins logs.\033[0m"
        }
        always {
            echo "📄 Build completed at: ${new Date()}"
        }
    }
}
