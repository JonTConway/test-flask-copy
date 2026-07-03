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
        withCredentials([usernamePassword(credentialsId: 'docker', 
                                          usernameVariable: 'DOCKER_USER', 
                                          passwordVariable: 'DOCKER_PASS')]){
          // Using powershell avoids the "echo password |" syntax bug on Windows
            powershell """
                \$secpasswd = ConvertTo-SecureString "$DOCKER_PASS" -AsPlainText -Force
                \$mycreds = New-Object System.Management.Automation.PSCredential ("$DOCKER_USER", \$secpasswd)
                Docker login --credential-current-user
                echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
            """
            
            bat 'docker push JonTConway/test-flask-copy:latest'
        }
      }
    }
  }
}
