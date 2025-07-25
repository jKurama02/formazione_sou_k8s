pipeline {
    agent {
        label 'jenkins-agent_1'
    }
    
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKER_IMAGE_NAME = 'anmedyns/my_app'
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
                    env.dockerTag = 'idk'  //like this you can overwrite the env.vaeriable
                    echo "GIT_BRANCH: ${env.GIT_BRANCH} ––––––––––––––––––––––––––––––––––––––"
                    
                    if (env.GIT_BRANCH == 'origin/master') {
                        env.dockerTag = 'latest'
                    } else if (env.GIT_BRANCH.startsWith('origin/tags/')) {
                        env.dockerTag = env.GIT_BRANCH.replace('origin/tags/', '')
                    } else if (env.GIT_BRANCH == 'origin/develop') {
                        env.dockerTag = "develop-${env.GIT_COMMIT.substring(0, 7)}"
                    }
                    echo "FINAL DOCKER TAG: ${env.dockerTag}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${env.DOCKER_IMAGE_NAME}:${env.dockerTag}", ".")  // "." current directory
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("${env.DOCKER_IMAGE_NAME}:${env.dockerTag}").push()
                    }
                }
            }
        }
    }
}
