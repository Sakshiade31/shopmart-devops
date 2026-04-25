pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Sakshiade31/shopmart-devops.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t shopmart:v2 .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker stop shopmart-container || true'
                sh 'docker rm shopmart-container || true'
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d -p 8080:80 --name shopmart-container shopmart:v2'
            }
        }
    }
}
