pipeline{
    agent any
    tools {
        maven 'M2_HOME'
    }
    environment {
        registry = '880385147960.dkr.ecr.us-east-1.amazonaws.com/geolocation_ecr_repo'
        registryCredential = 'ecr-credential'
        dockerimage = ''
    }
     stages{
         stage('Git Checkout') {
            steps{
             git branch: 'main', url: 'https://github.com/henrykrop2022/hospital_management.git'
             }
         }
        stage('Maven Build'){
            steps{
                 sh 'mvn clean package'
             }
         }
        stage('Unit Test'){
             steps{
                 sh 'mvn test'
             }
         }
        stage('Sonar Analysis'){
            steps{
                script{
                     withSonarQubeEnv(credentialsId: 'hospital-token') {
                sh 'mvn sonar:sonar'
                     }
                 }
             }
         }
        stage('Building Image'){
            steps{
                 script{
                     docker = docker.build registry 
                }
             }
         }
        stage('Pushing to ECR'){
            steps{
                script{
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 880385147960.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'docker push 880385147960.dkr.ecr.us-east-1.amazonaws.com/geolocation_ecr_repo:latest'     
                }
            }
        }
        stage ("Kube Deploy") {
            steps {
                withCredentials([kubeconfigFile(credentialsId: 'eks_credential', variable: 'KUBECONFIG')]) {
                 sh 'kubectl config use-context ducation-eks-1omkKCqq'
                 sh "kubectl apply -f eks_deploy_from_ecr.yaml"
                }   
            }
        }
    }
}
        
     
