pipeline {
    agent any
    environment {
        DOCKERHUB_USER = 'kapilanramesh'
        DOCKERHUB_PASS = 'Kapilan@2003'
    }
    stages {
        stage('Clone Source Code') {
            steps {
                git branch: 'main', url: 'https://github.com/<your-username>/<your-repo>.git'
            }
        }
        stage('Build Images') {
            steps {
                sh 'docker-compose build'
            }
        }
        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker tag backend:latest $USER/backend:latest'
                    sh 'docker tag frontend:latest $USER/frontend:latest'
                    sh 'docker push $USER/backend:latest'
                    sh 'docker push $USER/frontend:latest'
                }
            }
        }
        stage('Deploy on EC2') {
            steps {
                sh 'docker-compose down'
                sh 'docker-compose up -d'
            }
        }
    }
}
