pipeline {
    triggers {
    githubPush()
  }

  environment {
    dockerimagename = "harshithraj24/nodeapp"
    dockerImage = ""
    HELM_PATH = "/home/dell/clonednodeapp/deployment/charts"    
    RELEASE_NAME = "demo"
    NAMESPACE = "demospace"

  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/Harshithraj24/clonednodeapp'      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
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
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
       
    steps{
kubeconfig(caCertificate: '', credentialsId: 'kubejenkin', serverUrl: '') {
          sh "helm upgrade -i -n ${NAMESPACE} ${RELEASE_NAME} ${HELM_PATH} -f ${HELM_PATH}/values.yaml    }
    }
    }
    }
  }

