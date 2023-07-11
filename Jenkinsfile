pipeline {

  environment {
    DOCKER_IMAGE_NAME = "thetips4you/nodeapp"
    DOCKER_IMAGE = ""

  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/shazforiot/nodeapp_test.git'
      }
    }

    stage('Build Image') {
      steps{
        script {
          DOCKER_IMAGE = docker.build DOCKER_IMAGE_NAME
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            DOCKER_IMAGE.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}