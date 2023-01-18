pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = 'ECR repo url'
    registryCredential = 'jenkinsuser'
    dockerimage = ''
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/yemi-fagbuyi/Helloworld_22.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Images') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }  
    }
}
