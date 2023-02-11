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
                     sh 'docker image build -t $JOB_NAME:V1$BUILD_ID .'
                     sh ' docker image tag $JOB_NAME:V1$BUILD_ID henryrop/$JOB_NAME:V1$BUILD_ID'
                     sh ' docker image tag $JOB_NAME:V1$BUILD_ID henryrop/$JOB_NAME:latest'
                }
            }  
        }
         stage('Pushing to ecr') {
            steps{
                script {
                      withCredentials([string(credentialsId: 'henryrop', variable: 'dockerhub_Cred')]) {
                        sh 'docker login -u henryrop -p ${dockerhub_cred}'
                        sh 'docker image push henryrop/$JOB_NAME:V1$BUILD_ID'
                        sh  'docker image push henryrop/$JOB_NAME:latest'
                    }
                }
            }
        }
    }
}