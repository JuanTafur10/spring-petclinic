pipeline {
  agent any

  stages {
    stage('Maven Install') {
      agent {
        docker {
          image 'maven:3.9-eclipse-temurin-25'
          reuseNode true
        }
      }
      steps {
        sh 'mvn clean install'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t juantafur10/spring-petclinic:gestion-udem-jenkins .'
      }
    }

    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerHub',
          usernameVariable: 'dockerHubUser',
          passwordVariable: 'dockerHubPassword'
        )]) {
          sh 'echo "$dockerHubPassword" | docker login -u "$dockerHubUser" --password-stdin'
          sh 'docker push TU_USUARIO_DOCKERHUB/spring-petclinic:gestion-udem-jenkins'
        }
      }
    }
  }
}