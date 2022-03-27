pipeline {
    agent any

    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/SiriveruJagadeesh/Jenkins-pipeline.git'
            }
        }
       
        stage('Build Docker Image') {
            steps {
                script {
                    
                 sh 'docker build -t jagadeeshsiriveru/webimg:v1 .'
                }
            }
        }
        stage('Pushing Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'e61a8d5e-2543-4b62-88e7-9445c1052dbb', toolName: 'DOCKER') {
               sh 'docker push jagadeeshsiriveru/webimg:v1'
               
                }
              }
            }
          }
        
        stage('Run Container on EC2-INSTANCE'){
            steps {
                sshagent(['EC2-INSTANCE']) {
                sh "ssh -o StrictHostKeyChecking=no ec2-user@13.127.52.48 'docker run -p 4000:4000 -d jagadeeshsiriveru/webimg:v1'"
                }
                
                sshagent(['EC2-INSTANCE']) {
                sh "ssh -o StrictHostKeyChecking=no ec2-user@13.232.65.99 'docker run -p 4000:4000 -d jagadeeshsiriveru/webimg:v1'"
                  }
            }
        }
    }
}
