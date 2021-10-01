pipeline {
    agent any
    tools{
        maven 'apache-maven-3.8.2'
    }
    stages {
        stage('Get db url and change localhost') {
            steps {
                script{
                  withCredentials([string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID'),string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                     sh """
                     docker run --rm -e AWS_ACCESS_KEY_ID=\$AWS_ACCESS_KEY_ID \
                     -e AWS_SECRET_ACCESS_KEY=\$AWS_SECRET_ACCESS_KEY \
                     amazon/aws-cli rds describe-db-instances --region \
                     eu-central-1 --query DBInstances[0].Endpoint.Address > output.txt
                     """
                     sh """export DB_URL=\$(cut -d '"' -f 2 output.txt); \
                      sed -i "s/localhost/\$DB_URL/g" /var/jenkins_home/workspace/Build/src/main/resources/application-mysql.properties"""
                    }
                }
            }
        }
        stage('Build') {
            steps {
                script{
                  sh "./mvnw package"
                }
            }
        }
        stage('Build image') {
            steps {
                script{
                  sh """docker build -t 140625812000.dkr.ecr.eu-central-1.amazonaws.com/spring-petclinic .; \
                     docker run --rm -e AWS_ACCESS_KEY_ID=\$AWS_ACCESS_KEY_ID \
                     -e AWS_SECRET_ACCESS_KEY=\$AWS_SECRET_ACCESS_KEY \
                     amazon/aws-cli ecr get-login-password --region eu-central-1 | docker login --username AWS \
                      --password-stdin 140625812000.dkr.ecr.eu-central-1.amazonaws.com;
                      docker push 140625812000.dkr.ecr.eu-central-1.amazonaws.com/spring-petclinic:latest"""
                }
            }
        }
    }
}