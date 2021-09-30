pipeline {
    agent any
    stages {
        stage('Check aws') {
            steps {
                script{
                  sh "aws --version"
                }
            }
        }
    }
}