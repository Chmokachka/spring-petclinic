pipeline {
    agent any
    stages {
        stage('Check aws') {
            steps {
                script{
                  withCredentials([string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID'),string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                     docker run --rm -e AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} -e AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} amazon/aws-cli rds describe-db-instances --region eu-central-1 --query DBInstances[0].Endpoint.Address > output.txt
                     DB_URL=$(cut -d '"' -f 2 output.txt)
                     echo "$DB_URL"
                    }
                }
            }
        }
    }
}