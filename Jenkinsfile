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
                    docker.build("338591529130.dkr.ecr.us-east-1.amazonaws.com/netflix-oct")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 338591529130.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://338591529130.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:1', 'ecr:us-east-1:brian-ecr') {
                    // build image
                    def myImage = docker.build("338591529130.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:v1")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
