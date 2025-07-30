pipeline {
  agent any

  environment {
    IMAGE_NAME = "aarensh/jenkin_image"
    IMAGE_TAG = "IPST"
  }

  triggers {
    pollSCM('* * * * *')
  }

  stages {

    stage('Clone Repo') {
      steps {
        git url: "https://github.com/aarensh/IPST", 
            branch: "master",
            credentialsId: "Github_token"
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          // "." means Jenkins workspace, so ".frontend/" is wrong unless that's a subfolder in the root
          dockerImage = docker.build("${IMAGE_NAME}:${IMAGE_TAG}", "./frontend")
        }
      }
    }

    stage('Login and Push to DockerHub') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'Dockerhub_token') {
            dockerImage.push("${IMAGE_TAG}")
          }
        }
      }
    }

  } // end stages
}
