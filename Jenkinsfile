pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Lakshana5116/set-15-CI-CD.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t lakshana5116/set15-nginx:1.0 .'
            }
        }

        stage('Test Docker Image') {
            steps {
                bat 'docker run --rm lakshana5116/set15-nginx:1.0 nginx -t'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push lakshana5116/set15-nginx:1.0'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
                bat 'kubectl apply -f service.yaml'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'kubectl rollout status deployment/set15-web --timeout=60s'
                bat 'kubectl get deployment set15-web'
                bat 'kubectl get pods -l app=set15-web'
                bat 'kubectl get service set15-web-service'
            }
        }
    }
}