pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker:24.0.6
    tty: true
    securityContext:
      privileged: true
    volumeMounts:
    - name: docker-sock
      mountPath: /var/run/docker.sock
  volumes:
  - name: docker-sock
    hostPath:
      path: /var/run/docker.sock
"""
        }
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/nasarr2002/wordpress-k8s.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                container('docker') {
                    sh """
                        echo "🚀 Building image nas2/wordpress-k8s:v${BUILD_NUMBER}"
                        docker build -t nas2/wordpress-k8s:v${BUILD_NUMBER} .
                    """
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                container('docker') {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                                                     usernameVariable: 'USER',
                                                     passwordVariable: 'PASS')]) {
                        sh """
                            echo "🔐 Logging in Docker Hub..."
                            echo "$PASS" | docker login -u "$USER" --password-stdin
                        """
                    }
                }
            }
        }

        stage('Push Image') {
            steps {
                container('docker') {
                    sh """
                        echo "📤 Pushing image..."
                        docker push nas2/wordpress-k8s:v${BUILD_NUMBER}
                    """
                }
            }
        }
    }
}
