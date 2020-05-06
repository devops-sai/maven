pipeline {
    agent any
    stages {
        stage('compile') {
            steps {
                sh '''
                    mvn --version
                    mvn compile
                '''
            }
        }
        stage('test') {
            steps {
                sh '''
                    mvn --version
                    mvn test
                '''
            }
        }
        stage('package') {
            steps {
                sh '''
                    mvn --version
                    mvn package
                '''
            }
        }
    }
}