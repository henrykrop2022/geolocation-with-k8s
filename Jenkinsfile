pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
        registry = '880385147960.dkr.ecr.us-east-1.amazonaws.com/geolocation'
        dockerimage = '' 
    }
    stages{
        stage('checkout') {
            steps{
                git branch: 'main', url: 'https://github.com/henrykrop2022/geolocation-with-k8s.git'
            }
        }
         stage(' Code Build'){
            steps{
                sh 'mvn clean install -DskipTests=true'
                sh 'mvn package -DskipTests'
            }
        }
        stage(' SonarQube Analysis'){
            steps {
                script {
                withSonarQubeEnv(credentialsId: 'SonarQube-token') {
                sh 'mvn clean verify sonar:sonar'
                   }
                }
            }   
            
        }
        
        stage('Quality Gate'){
            steps{
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube-token'
                }
            }
        }
         stage('Building image') {
            steps{
                script {
                      dockerImage = docker.build registry
                }
            }  
        }
         stage('Docker to ECR') {
            steps{
                script {
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 880385147960.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'docker push 880385147960.dkr.ecr.us-east-1.amazonaws.com/geolocation:latest'
                }
            }
        } 
      
        stage ("EKS Deployment") {
            steps {
                script {
                      kubeconfig(credentialsId: 'eks_credential', serverUrl: '') {
                      sh "kubectl apply -f eks-deploy-from-ecr.yaml"
                   }
                 }
            }
        }
          stage ('Cleanup workspace'){
            steps{
                script {
                    cleanWs()
                }
            }
        }
    }
}
    
