pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("908778560637.dkr.ecr.us-east-1.amazonaws.com/netflix-app:v1")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 908778560637.dkr.ecr.us-east-1.amazonaws.com/netflix-app:v1'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://908778560637.dkr.ecr.us-east-1.amazonaws.com/netflix-app', 'ecr:us-east-1:patdada-ecr') {
                    // build image
                    def myImage = docker.build("908778560637.dkr.ecr.us-east-1.amazonaws.com/netflix-app:v1")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
