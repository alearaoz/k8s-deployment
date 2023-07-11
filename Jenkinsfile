pipeline {

  environment {
    DOCKER_IMAGE_NAME = "aaraoz/nodeapp"
    DOCKER_IMAGE = ""

  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/alearaoz/k8s-deployment'
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
               registryCredential = 'docker_aaraoz'
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
          kubernetesDeploy(configs: "deployment.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}