pipeline {
  agent any
  tools {
    maven 'Maven 3.8.1'
  }
  environment {
    DOCKER_IMAGE = "hn9602/spring-petclinic:${BUILD_NUMBER}"
  }
  stages {
    stage('Checkout Code') {
      steps {
        git credentialsId: 'github-pat', url: 'https://github.com/hi-man-shu-7/spring-petclinic.git'
      }
    }
    stage('Build with Maven') {
      steps {
        sh './mvnw clean package'
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $DOCKER_IMAGE .'
      }
    }
    stage('Push to Docker Hub') {
      steps {
        withDockerRegistry([credentialsId: 'docker-hub-creds']) {
          sh 'docker push $DOCKER_IMAGE'
        }
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl set image deployment/sample-app-deployment sample-container=$DOCKER_IMAGE'
      }
    }
  }
}
