pipeline{
    
    agent any
    
    tools{
        maven 'Maven3'
    }
    
    environment{
        DOCKERHUB_CREDENTIALS=credentials('DockerHub')
    }
    
    stages{
        
        stage('Checkout'){
            steps{
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'Git', url: 'https://github.com/GauravBluepi123/jenkins-kubernetes-example.git']]]) 
            }
        }
        
        stage('Build'){
            steps{
            sh 'docker build -t gauravkumarpandey/Java:latest .'
            }
        }
        
        stage('Docker Login..'){
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        
        stage('Push'){
            steps{
                sh 'docker push gauravkumarpandey/Java:latest'
            }
        }
    }
    
    post{
        always{
            sh 'docker logout'
        }
    }
    
}
