pipeline {
    agent any
    tools {
        nodejs '18.17.0'
    }
    environment {
        // Docker image name
        DOCKER_IMAGE = 'Patriciaog/CaPipelineApp:latest'
        
        // Docker registry URL
        // Example for Docker Hub: 'https://index.docker.io/v1/'
        DOCKER_REGISTRY = 'https://index.docker.io/v1/'
        
        // Docker registry credentials ID in Jenkins
        DOCKER_CREDENTIALS_ID = '5a952337-b627-4688-b26d-82263d81e7b1'
    }
    stages {
        stage('Clone Repository') {
            steps {
                // Repository URL
                git 'https://github.com/NorbertTuz/Ca-Ui-Express.git'
            }
        }

        stage('Build') {
            steps {
                sh "npm ci"
            }
        }

        stage('Unit Test') {
            steps {
                sh "npm run test:unit"
            }
        }

        stage('Integration Test') {
            steps {
                sh "npm run test:integration"
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry("${DOCKER_REGISTRY}", "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}").run("-d -p 80:3000")
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
