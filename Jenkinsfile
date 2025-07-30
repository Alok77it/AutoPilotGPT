pipeline{
  agent any
  environment {
    IMAGE_NAME = "aarensh/Jenkin_image"
    IMAGE_TAG = "IPST"
  }
  triggers {
    pollSCM('* * * * *')
  }
  stages {
    stage {
      steps {
        git url: "https://github.com/aarensh/IPST", 
            branch: "ipst",
            credentialsId: "Github_token"
        }
      }
    stage {
      steps {
        script {
          dockerImage = docker.build("${IMAGE_NAME}:${IMAGE_TAG}", ".frontend/" )
        }
      }
    }
      stage('Login and Push to DockerHub') {
        steps {
          script {
            docker.withRegistry('https://index.docker.io/v1/', 'dockerhub_credentials') {
                dockerImage.push("${IMAGE_TAG}")
            }
          }
        }
      }
  }
}
  
