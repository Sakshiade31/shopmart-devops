pipeline {
    agent any

    stages {

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

        stage('Health Check') {
            steps {
                script {
                    sleep(10)
                    def status = sh(script: "curl -s -o /dev/null -w '%{http_code}' http://172.17.0.1:8080", returnStdout: true).trim() 
                    
                    if (status != "200") {
                        error("Health check failed")
                    }
                }
            }
        }
    }

    post {
        failure {
            echo "Deployment failed! Rolling back..."

            sh 'docker stop shopmart-container || true'
            sh 'docker rm shopmart-container || true'

            sh 'docker run -d -p 8080:80 --name shopmart-container shopmart:v1'
        }
    }
}
