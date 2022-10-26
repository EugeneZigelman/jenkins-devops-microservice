pipeline {
  agent any
  // agent{ docker { image 'maven:3.6.3'} }
  // agent{ docker { image 'node:19.0'} }
  environment{
    dockerHome= tool 'myDocker'
    mavenHome= tool 'myMaven'
    PATH= "$dockerHome/bin:$mavenHome/bin:$PATH"
  }
  stages{
    stage('Checkout'){
      steps{
        echo "Build"
        sh 'mvn --version'
        sh 'docker version'
        echo "$PATH"
      }
    }
    stage('Compile'){
      steps{
        
        sh 'mvn clean compile'
      }
    }
    stage('Test'){
      steps{
      
        sh 'mvn test'
      
      }
    }
    stage('Integration Test'){
      steps{
        
        sh 'mvn failsafe:integration-test failsafe:verify'
      }
    }
    stage('Package'){
      steps{
        
        sh 'mvn package -DskipTests'
      }
    }
    stage('Build Docker Image'){
      steps{
        script{
          dockerImage=docker.build("eugenezigelman/currency-exchange-devops:{$env.BUILD_TAG}")
        }
      }
    }
    
    stage('Push Docker Image'){
      steps{
        script{
          docker.withRegistry('','dockerhub'){
            dockerImage.push();
            dockerImage.push('latest');
          }
        }
        
      }
    }
  }
  post{
    always{
      echo 'I run always'
    }
    success{
      echo 'I run when you are successful'
    }
    failure{
      echo 'I run when you are fail'
    }
  }
	
}
