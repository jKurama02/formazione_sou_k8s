pipeline {
    agent {
        label 'jenkins-agent_1'
    }
    
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKER_IMAGE_NAME = 'anmedyns/my_app'
        dockerTag = 'idk'
    }
    
    stages {
        stage('Cloning repository GitHub') {
            steps {
                git branch: 'main', url: 'https://github.com/jKurama02/formazione_sou_k8s.git'
            }
        }
        stage('Tag image') {
            steps {
                script {
                    // Determina il tag Docker in base al Git ref
                    if (env.GIT_BRANCH == 'origin/master') {
                        dockerTag = 'latest'
                    } else if (env.GIT_BRANCH.startsWith('origin/tags/')) {
                        dockerTag = env.GIT_BRANCH.replace('origin/tags/', '')
                    } else if (env.GIT_BRANCH == 'origin/develop') {
                        dockerTag = "develop-${env.GIT_COMMIT.substring(0, 7)}"
                    } else {
                        dockerTag = 'custom'
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE_NAME}:${dockerTag}")
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("${DOCKER_IMAGE_NAME}:${dockerTag}").push()
                    }
                }
            }
        }
    }
}
