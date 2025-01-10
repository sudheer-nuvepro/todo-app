pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sudheer99123/todo-taskapp"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sudheer-nuvepro/java_app.git'
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                    }
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                sh '''
                    docker stop todo-taskapp || true
                    docker rm todo-taskapp || true
                    docker run -d -p 8081:8081 --name todo-taskapp ${DOCKER_IMAGE}:${DOCKER_TAG}
                '''
            }
        }

        stage('Cleanup Workspace') {
            steps {
                sh 'rm -rf *'
            }
        }
    }
}
