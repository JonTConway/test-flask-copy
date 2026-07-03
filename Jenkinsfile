pipeline{
  agent any
  environment{
      IMAGE_NAME = 'JonTConway/test-flask-copy'
  }
  stages{
    stage('Checkout'){
      steps{
       git branch: 'main', url: 'https://github.com/JonTConway/test-flask-copy' 
      }
    }
    stage('Build Docker image'){
      steps{
        bat "docker build -t %IMAGE_NAME%:latest ."
      }
    }
    stage('Push to DockerHub'){
      steps{
        withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]){
          bat """
          echo %DOCKER_PASS% | 
          docker login -u %DOCKER_USER% --password-stdin 
          docker push %IMAGE_NAME%:latest
          docker logout
          """
        }
      }
    }
  }
}
