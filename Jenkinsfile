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
    - "--dockerfile=/workspace/docker/Dockerfile"
    - "--context=/workspace/docker"
    - "--destination=nasarr/wordpress-custom:v${BUILD_NUMBER}"
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
        stage('Build with Kaniko') {
            steps {
                echo "🔧 Building with Kaniko..."
            }
        }
    }
}
