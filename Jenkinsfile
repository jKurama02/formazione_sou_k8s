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
                git branch: 'main', url: 'https://github.com/jKurama02/formazione_sou_k8s/releases/tag/v1.0'
            }
        }
        stage('Tag image') {
            steps {
                script {
                    echo "GIT_BRANCH: ${env.GIT_BRANCH}"
                    if (env.GIT_BRANCH == 'origin/master') {
                        env.dockerTag = 'latest'
                    } else if (env.GIT_BRANCH.startsWith('origin/tags/')) {
                        env.dockerTag = env.GIT_BRANCH.replace('origin/tags/', '')
                    } else if (env.GIT_BRANCH == 'origin/develop') {
                        env.dockerTag = "develop-${env.GIT_COMMIT.substring(0, 7)}"
                    } 
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE_NAME}:${env.dockerTag}")
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("${DOCKER_IMAGE_NAME}:${env.dockerTag}").push()
                    }
                }
            }
        }
    }
}
