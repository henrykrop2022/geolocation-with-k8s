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
        stage('UNIT Testing'){
            steps{
                sh 'mvn clean install package'
            }
        }
    }
}