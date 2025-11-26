pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:latest
    command:
    - /kaniko/executor
    args:
    - "--dockerfile=Dockerfile"
    - "--context=/workspace/"
    - "--destination=nasarr/wordpress-custom:v${BUILD_NUMBER}"
    - "--verbosity=debug"
    volumeMounts:
    - name: docker-config
      mountPath: /kaniko/.docker
  volumes:
  - name: docker-config
    secret:
      secretName: docker-config
"""
        }
    }

    stages {

        stage('Build with Kaniko') {
            steps {
                echo "Building image using Kaniko..."
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    sh """
                        helm upgrade --install wordpress-stack ./helm/wordpress-stack \
                            --namespace default \
                            --set wordpress.tag=v${BUILD_NUMBER}
                    """
                }
            }
        }
    }
}
