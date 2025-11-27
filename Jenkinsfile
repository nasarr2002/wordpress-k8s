pipeline {

   agent {
    kubernetes {
        yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: jenkins-agent
spec:
  serviceAccountName: default
  containers:
    - name: docker
      image: docker:24.0.6-dind
      securityContext:
        privileged: true
      tty: true
      command: [ "dockerd-entrypoint.sh" ]
      args: [ "--host=tcp://0.0.0.0:2375", "--host=unix:///var/run/docker.sock" ]
      volumeMounts:
        - name: dind-storage
          mountPath: /var/lib/docker

    - name: kubectl
      image: lachlanevenson/k8s-helm:latest
      command: [ "cat" ]
      tty: true

  volumes:
    - name: dind-storage
      emptyDir: {}
"""
        defaultContainer 'docker'
    }
}

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = "nas20/wordpress-k8s"
        IMAGE_TAG = "v1"
    }

    stages {

        stage('Clone repository') {
            steps {
                container('docker') {
                    git branch: 'main',
                        credentialsId: 'github-creds',
                        url: 'https://github.com/nasarr2002/wordpress-k8s.git'
                }
            }
        }

        stage('Build Docker image') {
            steps {
                container('docker') {
                    sh "sleep 5"
                    sh """
                        docker build -t $IMAGE_NAME:$IMAGE_TAG -f docker/Dockerfile docker/
                    """
                }
            }
        }

        stage('Login & Push to Docker Hub') {
            steps {
                container('docker') {
                    sh "echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u \$DOCKERHUB_CREDENTIALS_USR --password-stdin"
                    sh "docker push $IMAGE_NAME:$IMAGE_TAG"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                container('kubectl') {

                    withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG_FILE')]) {
                        sh """
                          mkdir -p /home/jenkins/.kube
                          cp $KUBECONFIG_FILE /home/jenkins/.kube/config
                          chmod 600 /home/jenkins/.kube/config
                        """
                    }

                    sh """
                      export KUBECONFIG=/home/jenkins/.kube/config
                      helm upgrade --install blog ./blog -n default \
                        --set image.repository=$IMAGE_NAME \
                        --set image.tag=$IMAGE_TAG
                    """
                }
            }
        }
    }
}
