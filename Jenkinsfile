 pipeline {
  environment {
    registry = "harsha0961/online_bus"
    registryCredential = 'docker_hub_harsha0961'
    dockerImage = ''
  }
  agent any
  stages{
  
    stage ('Build') {
      steps{
        echo "Building Project"
        
         sh 'npm install'
         sh 'npm run build'
         sh "npm run ng --build  --prod"
        }
      }
    
    
    
    
    stage ('Build Docker Image') {
      steps{
        echo "Building Docker Image"
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage ('Push Docker Image') {
      steps{
        echo "Pushing Docker Image"
        script {
          docker.withRegistry( '', registryCredential ) {
              dockerImage.push()
              dockerImage.push('latest')
          }
        }
      }
    }
     stage ('Deploy to Dev') {
      steps{
        echo "Deploying to Dev Environment"
        sh "docker rm -f online_bus || true"
        sh "docker run -d --name=online_bus -p 8081:8080 harsha0961/online_bus"
      }
    }
  }
}
