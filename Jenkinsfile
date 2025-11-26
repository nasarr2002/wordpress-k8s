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
    args:
      - "--dockerfile=Dockerfile"
      - "--context=git://github.com/nasarr2002/wordpress-k8s.git"
      - "--destination=nasarr/wordpress-custom:v${BUILD_NUMBER}"
      - "--verbosity=info"
    volumeMounts:
    - name: kaniko-secret
      mountPath: /kaniko/.docker
  volumes:
  - name: kaniko-secret
    secret:
      secretName: dockerhub-creds
"""
        }
    }

    stages {
        stage('Build & Push Image with Kaniko') {
            steps {
                echo "Building Docker image with Kaniko..."
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
