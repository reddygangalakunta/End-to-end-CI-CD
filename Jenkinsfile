pipeline {
    agent any
    
    environment {
        DOCKER_HUB_USERNAME = 'yashwanthreddy1232'
        DOCKER_HUB_PASSWORD = 'Yogeshwanth@2001'
        IMAGE_NAME = 'flask-app'
        IMAGE_TAG = 'latest'
        GIT_REPO = 'https://github.com/reddygangalakunta/End-to-end-CI-CD.git'
        ARGOCD_APP_NAME = 'flask-app'
        ARGOCD_SERVER = 'http://argocd-server:8080'
    }

    stages {
        stage('Clone GitHub Repository') {
            steps {
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_HUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_HUB_USERNAME}") {
                        docker.image("${DOCKER_HUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}").push()
                    }
                }
            }
        }

        stage('Trigger ArgoCD Sync') {
            steps {
                script {
                    sh """
                    argocd login ${ARGOCD_SERVER} --username admin --password your-argocd-password --insecure
                    argocd app sync ${ARGOCD_APP_NAME}
                    """
                }
            }
        }
    }
}
