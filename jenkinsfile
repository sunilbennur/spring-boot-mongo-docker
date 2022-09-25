pipeline {
  agent any
  
  stages {
    
    stage("git clone"){
      steps {
        git 'https://github.com/sunilbennur/spring-boot-mongo-docker.git'
      }
    }
    stage("Maven clean build") {
      steps { 
        script {
            def mavenHome = tool name:"Maven-3.8.6", type: "maven"
            def mavenCMD = "${mavenHome}/bin/mvn"
            sh "${mavenCMD} clean package"
        }
       }
    }
    stage("Build Docker Image") {
      steps {
        sh "docker build -t sunilbennur/spring-boot-mango1 ."
      }
   }
    stage("Docker push") {
      steps {
        withCredentials([string(credentialsId: 'DOCKER_HUB_CREDENTIALS', variable: 'DOCKER_HUB_CREDENTIALS')]) {
        sh "docker login -u sunilbennur -p ${DOCKER_HUB_CREDENTIALS}"
        }
        sh "docker push sunilbennur/spring-boot-mango1 "
       }
    }
    stage("Deploy application in k8s kluster") {
      steps {
        sh 'kubectl apply -f springBootMongo.yml'
      }
    }
  }
}
