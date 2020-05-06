pipeline {
    agent any
    stages {
        stage('compile') {
            steps {
                cleanWs()
                sh '''
                    mvn --version
                    mvn compile
                '''
            }
        }
        stage('test') {
            steps {
                cleanWs()
                sh '''
                    mvn --version
                    mvn test
                '''
            }
        }
        stage('package') {
            steps {
                cleanWs()
                sh '''
                    mvn --version
                    mvn package
                '''
            }
        }
    }
}