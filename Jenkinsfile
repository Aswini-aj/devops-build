pipeline {
    agent any

    environment {``
        DOCKER_DEV_REPO = "aswini-aj/dev"
        DOCKER_PROD_REPO = "aswini-aj/prod"
        DOCKER_CREDENTIALS_ID = "aswini05"
        GITHUB_CREDENTIALS_ID = "Aswini-aj"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    def imageTag = "${env.BUILD_ID}"
                    docker.build("${env.DOCKER_DEV_REPO}:${imageTag}")
                }
            }
        }

        stage('Push Docker Image to Dev') {
            when {
                branch 'dev'
            }
            steps {
                script {
                    docker.withRegistry('', env.DOCKER_CREDENTIALS_ID) {
                        def imageTag = "${env.BUILD_ID}"
                        docker.image("${env.DOCKER_DEV_REPO}:${imageTag}").push()
                    }
                }
            }
        }

        stage('Push Docker Image to Prod') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('', env.DOCKER_CREDENTIALS_ID) {
                        def imageTag = "${env.BUILD_ID}"
                        docker.image("${env.DOCKER_PROD_REPO}:${imageTag}").push()
                    }
                }
            }
        }
    }
}
