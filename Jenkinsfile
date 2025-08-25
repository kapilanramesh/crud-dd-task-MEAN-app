pipeline {
    agent any
    environment {
        DOCKERHUB_USER = 'kapilanramesh'
        DOCKERHUB_PASS = 'Kapilan@2003'
    }
    stages {
        stage('Clone Source Code') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/kapilanramesh/crud-dd-task-mean-app',
                credentialsId: 'github-cred'
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
                    sh 'docker tag kapilanramesh/my-backend $USER/my-backend'
                    sh 'docker tag kapilanramesh/my-frontend $USER/my-frontend'
                    sh 'docker push $USER/my-backend'
                    sh 'docker push $USER/my-frontend'
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
