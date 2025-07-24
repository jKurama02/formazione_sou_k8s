pipeline {
    agent {
        label 'jenkins-agent_1'
    }
    
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKER_IMAGE_NAME = 'anmedyns/my_app'
        DOCKER_TAG = 'custom'  // Valore di default
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
                    // Debug: stampa il branch per verificare
                    echo "GIT_BRANCH: ${env.GIT_BRANCH}"
                    
                    if (env.GIT_BRANCH == 'origin/master') {
                        env.DOCKER_TAG = 'latest'
                    } else if (env.GIT_BRANCH?.startsWith('origin/tags/')) {
                        env.DOCKER_TAG = env.GIT_BRANCH.replace('origin/tags/', '')
                    } else if (env.GIT_BRANCH == 'origin/develop') {
                        env.DOCKER_TAG = "develop-${env.GIT_COMMIT.substring(0, 7)}"
                    }
                    // Se nessuna condizione è soddisfatta, rimane 'custom'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE_NAME}:${env.DOCKER_TAG}")
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("${DOCKER_IMAGE_NAME}:${env.DOCKER_TAG}").push()
                    }
                }
            }
        }
    }
}
