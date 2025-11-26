pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kaniko
    image: registry.gitlab.com/kaniko-project/executor:debug
    command:
    - /busybox/sh
    args:
    - -c
    - |
      /kaniko/executor \
        --context=/workspace/wordpress-ci \
        --dockerfile=/workspace/wordpress-ci/docker/Dockerfile \
        --destination=nas20/wordpress-k8s:v${BUILD_NUMBER} \
        --verbosity=debug
    volumeMounts:
    - name: docker-config
      mountPath: /kaniko/.docker

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
                echo "🔥 Building & pushing Docker image nas20/wordpress-k8s:v${BUILD_NUMBER}"
                container("kaniko") {
                    sh "echo 🚀 Kaniko build started..."
                }
            }
        }
    }
}
