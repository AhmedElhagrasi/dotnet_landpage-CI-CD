pipeline {
    agent {        label 'master_node'    }
    
    environment {
        greating = "hello form jenkins s 🥰  "
    }
    
    stages {
        stage('build') {
            steps {
                git branch: 'main', credentialsId: 'e24508c4-03d7-4db0-ad4b-8f62e84c97e5', url: 'https://github.com/AhmedElhagrasi/dotnet_landpage-CI-CD.git'
                sh "docker build -t ahmedelhagrasi/flask_app:$BUILD_NUMBER ."
                 withCredentials([usernamePassword(credentialsId: '42601bb2-e45d-4ce2-91ef-18e26215ffd6', passwordVariable: 'pass', usernameVariable: 'user')]) {    
                sh "docker login -u $USER -p $PASS"
                sh "docker push ahmedelhagrasi/dotnet_landpage-CI-CD:$BUILD_NUMBER"
}
            }
        }
            
        stage('test'){
            steps{
                echo "no test cases found ..."
            }
        }

        
        stage('deploy'){
            steps{
                sh "docker run -d -p 500$BUILD_NUMBER:5000 ahmedelhagrasi/dotnet_landpage-CI-CD:$BUILD_NUMBER"
            }
        }
    }
}
// to setup webhook : https://320969dc648b.ngrok-free.app/github-webhook/
