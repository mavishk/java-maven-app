pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'mavishek/demo'
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials'
    }

    tools {
        maven 'Maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/<mavishk>/java-maven-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDENTIALS_ID) {
                        def image = docker.build("${DOCKER_IMAGE}:latest")
                        image.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                ansiblePlaybook(
                    playbook: 'ansible/deploy.yaml'
                )
            }
        }
    }
}
