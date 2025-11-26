pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kaniko
    image: docker.io/kaniko-project/executor:v1.23.2
    args:
    - "--dockerfile=/workspace/wordpress-ci/docker/Dockerfile"
    - "--context=/workspace/wordpress-ci"
    - "--destination=nas20/wordpress-k8s:v${BUILD_NUMBER}"
    - "--verbosity=debug"
    volumeMounts:
    - name: docker-config
      mountPath: /kaniko/.docker/
  volumes:
  - name: docker-config
    secret:
      secretName: docker-config
      items:
      - key: .dockerconfigjson
        path: config.json
"""
        }
    }

    stages {
        stage("Build with Kaniko") {
            steps {
                echo "🔥 Building & pushing image to Docker Hub repository nas20/wordpress-k8s..."
            }
        }
    }
}
