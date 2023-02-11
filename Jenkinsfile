pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
        registry = '880385147960.dkr.ecr.us-east-1.amazonaws.com/geolocation_ecr_rep'
        // registryCredential = 'aws-jenkis-ID'
        // docker_user_PSW = 'DOCKER_USER'
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
                      dockerImage = docker.build registry
                }
            }  
        }
         stage('Pushing to ECR') {
            steps{
                script {
                     sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS password-stdin 880385147960.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'docker push 880385147960.dkr.ecr.us-east-2.amazonaws.com/geolocation_ecr_rep:latest'
                    }
                }
            }
        }
    }
