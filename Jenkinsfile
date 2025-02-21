pipeline {
    agent any
    tools {
        maven 'Maven' // Ensure 'Maven' is configured in Jenkins Global Tool Configuration
    }
    environment {
        DOCKER_IMAGE = 's5carles/del:1'
        DOCKER_USERNAME = 's5carles' // Define your Docker Hub username here
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Angecalais97/handson'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $DOCKER_IMAGE ."
                }
            }
        }
        stage('Login to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'id', variable: 'DOCKER_PASSWORD')]) {
                    script {
                        sh "echo $DOCKER_PASSWORD | docker login --username $DOCKER_USERNAME --password-stdin"
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push $DOCKER_IMAGE"
                }
            }
        }
    }
}
