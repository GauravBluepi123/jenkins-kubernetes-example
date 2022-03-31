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
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'Git', url: 'https://github.com/GauravBluepi123/jenkins-kubernetes-example']]]) 
            }
        }
        
        stage('Build'){
            steps{
            sh 'docker build -t gauravkumarpandey/javaproject:latest .'
            }
        }
        
        stage('Docker Login..'){
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        
        stage('Push'){
            steps{
                sh 'docker push gauravkumarpandey/javaproject:latest'
            }
        }
        
        stage('Push to k8s'){
            steps{
                //sh 'kubectl apply -f .'
                kubernetesDeploy(configs: "nodejsapp.yml", kubeconfigId: "kubernetes")
            }
        }
    }
    
    post{
        always{
            sh 'docker logout'
        }
    }
    
}
