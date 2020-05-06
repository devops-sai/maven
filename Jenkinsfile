pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '3'))
    }
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
                sh 'xyz'
            }
        }
        // stage('cleanWS') {
        //     steps{
        //         cleanWs()
        //     }
        // }
    }
    post{
        always{
            cleanWs()
        }
        failure{
            echo "Build failed sending mail to devops team"
        }
    }
}