pipeline {
    // agent any
    agent {
        label 'Mac'
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '3'))
    }
    environment {
        // DISABLE_AUTH = 'true'
        JAVA_APPLICATION   = 'singlemodule'
    }
    parameters {
        booleanParam(defaultValue: true, description: 'you can uncheck the package if it should skip the package creation', name: 'package')
    }
    stages {
        stage('Build') {
            steps {
                echo "App is ${JAVA_APPLICATION}"
                // echo "DISABLE_AUTH is ${JAVA_APPLICATION}"
                sh 'printenv'
            }
        }
        stage('clean') {
            environment {
                JAVA_APPLICATION   = 'xyz'
            }
            steps {
                sh '''
                    mvn --version
                    mvn clean
                    echo ${JAVA_APPLICATION}
                '''
            }
        }
        stage('compile') {
            steps {
                sh 'mvn compile'
                echo "${JAVA_APPLICATION}"
            }
        }
        stage('test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('package') {
            when {
                // Only say hello if a "greeting" is requested
                expression { params.package == true }
            }
            steps {
               echo "package: ${params.package}"
               echo "JAVA_APPLICATION: ${env.JAVA_APPLICATION}"
                sh 'mvn package'
            }
        }
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