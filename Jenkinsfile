pipeline{
  agent any
  environment{
      IMAGE_NAME = 'jontconway/test-flask-copy'
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
        withCredentials([usernamePassword(credentialsId: 'docker', 
                                          usernameVariable: 'DOCKER_USER', 
                                          passwordVariable: 'DOCKER_PASS')]){
          // This safely logs in on Windows without using the pipe (|) character
            bat 'docker login -u %DOCKER_USER% --password %DOCKER_PASS%'
            
            // Push your image
            bat 'docker push %IMAGE_NAME%:latest'
        }
      }
    }
  }
}
