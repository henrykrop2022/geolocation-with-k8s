pipeline{
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '880385147960.dkr.ecr.us-east-1.amazonaws.com/geolocation'
    registryCredential = 'aws-jenkis'
    dockerimage = ''
        }
    stages{ 
        stage('checkout') {
            steps{
                git branch: 'main', url: 'https://github.com/henrykrop2022/geolocation-with-k8s.git'
            }
        }
        stage('UNIT Testing'){
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
    }
}