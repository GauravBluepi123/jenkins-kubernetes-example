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
        
        stage('Deploy to k8s'){
            steps{
                sshagent(['k8s']){
                    sh 'scp -r -o StrictHostKeyChecking=no nodejsapp.yaml bluepi@192.168.49.2:/home/bluepi'
            
                    scripts{
                            try{
                                sh 'kubectl apply -f /home/bluepi/nodejsapp.yaml --kubeconfig=/home/bluepi/kube.yaml'
                            }
                            catch(error){
                    
                            }
                   }
               }
            }
        }
    }
    
    post{
        always{
            sh 'docker logout'
        }
    }
    
}
