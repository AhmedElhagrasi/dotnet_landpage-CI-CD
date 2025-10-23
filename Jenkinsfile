pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ahmedelhagrasi/dotnet-landpage"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build .NET app') {
            steps {
                echo "Restoring and building .NET app..."
                sh 'dotnet restore'
                sh 'dotnet publish -c Release -o out'
            }
        }

        stage('Build Docker image') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }

        stage('Run Container Test') {
            steps {
                echo "Testing Docker container..."
                sh "docker run -d -p 5000:80 --name test_app$BUILD_NUMBER ${DOCKER_IMAGE}:${DOCKER_TAG}"
                sh "sleep 5"
                sh "docker ps"
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo "Pushing image to Docker Hub..."
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }

        stage('Cleanup') {
            steps {
                echo "Cleaning up containers and images..."
                sh 'docker rm -f test_app || true'
                sh 'docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG} || true'
            }
        }
    }
}
