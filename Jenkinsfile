pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-app'
        DOCKER_USER = 'kumarkoppisetti'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/kumarkoppisetti/kumar-p.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_USER/$IMAGE_NAME:latest .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $DOCKER_USER/$IMAGE_NAME:latest'
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
