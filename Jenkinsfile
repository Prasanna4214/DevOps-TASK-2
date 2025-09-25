pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "prasanna1881/single-pipeline-app"
        DOCKER_TAG = "${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Prasanna4214/DevOps-TASK-2.git'
            }
        }
        stage('Build') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }
        stage('Test') {
            steps {
                sh 'npm install || true'
                sh 'npm test || true'
            }
        }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred',
                                                 usernameVariable: 'DOCKER_USER',
                                                 passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                  docker stop single_app || true
                  docker rm single_app || true
                  docker run -d --name single_app -p 80:3000 ${DOCKER_IMAGE}:${DOCKER_TAG}
                '''
            }
        }
    }
}
