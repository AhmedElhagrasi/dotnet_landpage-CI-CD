pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ahmedelhagrasi/dotnet_landpage"
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

       

       

        

 	stage('Push to Docker Hub') {
            steps {
                echo "Pushing image to Docker Hub..."
		withCredentials([usernamePassword(credentialsId: '42601bb2-e45d-4ce2-91ef-18e26215ffd6', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    
                sh "docker login -u $USER -p $PASS"
                sh " docker push ${DOCKER_IMAGE}:${DOCKER_TAG} "
		}
		 
            }
        }

	 stage('Run Container Test') {
            steps {
                echo "Testing Docker container..."
                sh "docker run -d -p 6000:5001 --name dotnet_landpage ${DOCKER_IMAGE}:${DOCKER_TAG}"
                sh "sleep 5"
                sh "docker ps"
            }
        }

stage('Cleanup') {
            steps {
                echo "Cleaning up containers and images..."
               // sh 'docker rm -f dotnet_landpage || true'
               // sh 'docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG} || true'
            }
        }

    }
}
