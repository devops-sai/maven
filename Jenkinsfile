pipeline {
    agent any
    stages {
        stage('clean') {
            steps {
                sh '''
                    mvn --version
                    mvn clean
                '''
            }
        }
        stage('compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('package') {
            steps {
                sh 'mvn package'
            }
        }
        stage('cleanWS') {
            steps{
                cleanWS()
            }
        }
    }
}