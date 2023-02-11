pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
        registry = '880385147960.dkr.ecr.us-east-1.amazonaws.com/geolocation_ecr_rep'
        registryCredential = 'docker-Cred'
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
                sh 'mvn clean'
                sh 'mvn install -DskipTests'
                sh 'mvn package -DskipTests'
            }
        }
         stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }  
        }
         stage('Pushing to ecr') {
            steps{
                script {
                      withCredentials([usernameColonPassword(credentialsId: 'henryrop', variable: 'docker-Cred')]) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}