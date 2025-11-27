pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        KUBECONFIG_CRED = credentials('kubeconfig')
        IMAGE_NAME = "nas20/wordpress-k8s"
        IMAGE_TAG = "v1"
    }

    stages {
        stage('Clone repository') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-creds',
                    url: 'https://github.com/nasarr2002/wordpress-k8s.git'
            }
        }

        stage('Build Docker image') {
            steps {
                script {
                    sh """
                    docker build -t $IMAGE_NAME:$IMAGE_TAG -f docker/Dockerfile .
                    """
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                sh """
                echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                """
            }
        }

        stage('Push image') {
            steps {
                sh """
                docker push $IMAGE_NAME:$IMAGE_TAG
                """
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Create temporary kubeconfig
                    writeFile file: 'kubeconfig', text: KUBECONFIG_CRED
                    sh 'export KUBECONFIG=kubeconfig'

                    // Helm upgrade WordPress
                    sh """
                    helm upgrade --install blog ./blog -n default \
                        --set image.repository=$IMAGE_NAME \
                        --set image.tag=$IMAGE_TAG
                    """
                }
            }
        }
    }
}
