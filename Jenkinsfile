pipeline {
  agent {
    kubernetes {
      cloud 'SLKE'
      defaultContainer 'trivy'
      yaml """
kind: Pod
spec:
  containers:
  - name: trivy
    image: aquasec/trivy:0.19.1
    command: ['cat']
    tty: true
    resources:
      limits:
        memory: "100Mi"
        cpu: "100m"
    volumeMounts:
    - name: trivy
      mountPath: /root/.cache/trivy
  volumes:
  - name: trivy
    persistentVolumeClaim:
      claimName: trivy-claim
"""
    }
  }
  
  environment {
    IMAGE = "adoptopenjdk/openjdk11:alpine-jre"
  }

  stages {
     stage ('Image Scan') {
      steps {
        sh """
        trivy image --severity MEDIUM,HIGH,CRITICAL --exit-code 1 ${IMAGE}
        """
      }
    }
  } 
}
