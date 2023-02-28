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
                sh 'mvn test'
            }
        }
    }
        stage('Integration Testing'){
            steps{
                script{
                    sh 'mvn verify -DskipUnitTests=false'
                }
                }
            }
        }
    //     stage('Maven Build'){
    //         steps{
    //             sh 'mvn clean install package'
    //         }
    //     }
    //     stage(' SonarQube Analysis'){
    //         steps {
    //             script {
    //             withSonarQubeEnv(credentialsId: 'SonarQube-token') {
    //             sh 'mvn clean verify sonar:sonar'
    //                }
    //             }
    //         }       
    //     }
    //     stage('Quality Gate'){
    //         steps{
    //             script {
    //                 waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube-token'
    //             }
    //         }
    //    }
    //      stage('Building image') {
    //         steps{
    //             script {
    //                   dockerImage = docker.build registry
    //             }
    //         }  
    //     }
    //      stage('Docker to ECR') {
    //         steps{
    //             script {
    //                 sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 880385147960.dkr.ecr.us-east-1.amazonaws.com'
    //                 sh 'docker push 880385147960.dkr.ecr.us-east-1.amazonaws.com/geolocation:latest'
    //             }
    //         }
    //     } 
      
    //     stage ("EKS Deployment") {
    //         steps {
    //             script {
    //                   kubeconfig(credentialsId: 'eks_credential', serverUrl: '') {
    //                   sh "kubectl apply -f eks-deploy-from-ecr.yaml"
    //                }
    //              }
    //         }
    //     }
    //       stage ('Cleanup workspace'){
    //         steps{
    //             script {
    //                 cleanWs()
    //             }
    //         }
    //     }
    // }

    
