pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '3'))
    }
    // environment {
    //     JAVA_APPLICATION   = 'singlemodule'
    // }
    // parameters {
    //     booleanParam(defaultValue: true, description: 'you can uncheck the package if it should skip the package creation', name: 'package')
    // }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3.6.3-jdk-8'
                    args  '-u root -v /tmp:/tmp -v /Users/sairam/.m2:/root/.m2'
                    reuseNode true
                }
            }
            steps {
                // echo "App is ${JAVA_APPLICATION}"
                sh 'printenv'
            }
        }
        stage('clean') {
            // environment {
            //     JAVA_APPLICATION   = 'xyz'
            // }
             agent {
                docker {
                    image 'maven:3.6.3-jdk-8'
                    args  '-u root -v /tmp:/tmp -v /Users/sairam/.m2:/root/.m2'
                    reuseNode true
                }
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
             agent {
                docker {
                    image 'maven:3.6.3-jdk-8'
                    args  '-u root -v /tmp:/tmp -v /Users/sairam/.m2:/root/.m2'
                    reuseNode true
                }
            }
            steps {
                sh 'mvn compile'
                // echo "${JAVA_APPLICATION}"
            }
        }
        stage('test') {
             agent {
                docker {
                    image 'maven:3.6.3-jdk-8'
                    args  '-u root -v /tmp:/tmp -v /Users/sairam/.m2:/root/.m2'
                    reuseNode true
                }
            }
            steps {
                sh 'mvn test'
            }
        }
        stage('package') {
             agent {
                docker {
                    image 'maven:3.6.3-jdk-8'
                    args  '-u root -v /tmp:/tmp -v /Users/sairam/.m2:/root/.m2'
                    reuseNode true
                }
            }
            steps {
            //    echo "JAVA_APPLICATION: ${env.JAVA_APPLICATION}"
                sh 'mvn package'
            }
        }
        stage('dockerBuild') {
             agent {
                docker {
                    image 'docker:dind'
                    args  '-u root -v /tmp:/tmp -v /var/run/docker.sock:/var/run/docker.sock'
                    reuseNode true
                }
            }
            steps {
                sh 'docker build -t sairam1007/sample:1.0.1 .'
            }
        }
           stage('dockerPush') {
             agent {
                docker {
                    image 'docker:dind'
                    args  '-v /tmp:/tmp -v /var/run/docker.sock:/var/run/docker.sock'
                    reuseNode true
                }
            }
            steps {
                sh 'docker push sairam1007/sample:1.0.1'
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