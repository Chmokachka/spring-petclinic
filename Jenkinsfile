pipeline {
    agent any
    stages {
        stage('Check aws') {
            steps {
                script{
                  withCredentials([string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID'),string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AAWS_SECRET_ACCESS_KEY')]) {
                     sh "docker run --rm -it -e AWS_ACCESS_KEY_ID=AKIASBPPTTIQLITEI5BE -e AWS_SECRET_ACCESS_KEY=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 amazon/aws-cli rds describe-db-instances --region eu-central-1 --query DBInstances[*].Endpoint.Address"
                    }
                }
            }
        }
    }
}