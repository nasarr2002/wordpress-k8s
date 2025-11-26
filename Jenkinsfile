pipeline {
    agent any

    environment {
        REGISTRY = "docker.io"
        IMAGE_NAME = "nasarr/wordpress-custom"
        IMAGE_TAG = "v${BUILD_NUMBER}"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Scan Vulnerabilities') {
            steps {
                script {
                    sh "trivy image --exit-code 0 ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                                                 usernameVariable: 'DOCKER_USER',
                                                 passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }

        stage('Deploy to Kubernetes (staging)') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    sh """
                        helm upgrade --install wordpress-stack ./helm/wordpress-stack \
                            --namespace default \
                            --set wordpress.tag=${IMAGE_TAG}
                    """
                }
            }
        }
    }
}

