pipeline {
    agent any
    environment {
        DOCKERHUB_USER = 'kapilanramesh'
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
                sh 'docker build -t $DOCKERHUB_USER/my-backend ./backend'
                sh 'docker build -t $DOCKERHUB_USER/my-frontend ./frontend'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $USER/my-backend'
                    sh 'docker push $USER/my-frontend'
                }
            }
        }

        stage('Deploy on EC2') {
            steps {
                sh 'docker-compose down || true'
                sh 'docker-compose up -d'
            }
        }
    }
}
