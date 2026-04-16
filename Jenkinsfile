pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "aaronalpho14/html-demo"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/aaronalphons14/AgileLabAssessment4.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE% .'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    bat '''
                    docker login -u %USER% -p %PASS%
                    docker push %DOCKER_IMAGE%
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(
                    credentialsId: 'kubeconfig',
                    variable: 'KUBECONFIG'
                )]) {
                    bat '''
                    set KUBECONFIG=%KUBECONFIG%
                    kubectl apply -f deployment.yaml --validate=false
                    kubectl rollout restart deployment html-demo
                    '''
                }
            }
        }
    }
}
