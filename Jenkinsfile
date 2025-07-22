
pipeline {
    agent any
    
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKER_IMAGE_NAME = 'anmedyns/my_app'
        DOCKER_IMAGE_TAG = 'latest-idk'
    }
    
    stages {
        stage('Git clone') {
            steps {
                echo 'Cloning repository GitHub'
                git branch: 'main', url: 'https://github.com/tuousername/formazione_sou_k8s.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                    docker.build("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}")
            }
        }
    }
}