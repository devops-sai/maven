pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '3'))
    }
    environment {
    registry = "sairam1007/sample"
    registryCredential = 'docker-hub'
    dockerImage1 = ''
    dockerImage2 = ''
    BUILD_NUMBER = "1.0.1"
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
        // stage('compile') {
        //      agent {
        //         docker {
        //             image 'maven:3.6.3-jdk-8'
        //             args  '-u root -v /tmp:/tmp -v /Users/sairam/.m2:/root/.m2'
        //             reuseNode true
        //         }
        //     }
        //     steps {
        //         sh 'mvn compile'
        //         // echo "${JAVA_APPLICATION}"
        //     }
        // }
        // stage('test') {
        //      agent {
        //         docker {
        //             image 'maven:3.6.3-jdk-8'
        //             args  '-u root -v /tmp:/tmp -v /Users/sairam/.m2:/root/.m2'
        //             reuseNode true
        //         }
        //     }
        //     steps {
        //         sh 'mvn test'
        //     }
        // }
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
        stage("Parallel Docker Builds"){
            failFast true
            parallel{
                stages{
                    stage('dockerBuild1') {
                        agent {
                            docker {
                                image 'docker:dind'
                                args  '-u root -v /tmp:/tmp -v /var/run/docker.sock:/var/run/docker.sock'
                                reuseNode true
                            }
                        }
                        steps {
                            // sh 'docker build -t sairam1007/sample:1.0.2 .'
                            script {
                                dockerImage1 = docker.build registry + ":1.0.2"
                            }
                        }
                    }
                    stage('dockerPush1') {
                        agent {
                            docker {
                                image 'docker:dind'
                                args  '-v /tmp:/tmp -v /var/run/docker.sock:/var/run/docker.sock:rw -u root'
                                reuseNode true
                            }
                        }
                        steps {
                            script {
                                docker.withRegistry( '', registryCredential ) {
                                    dockerImage1.push()
                                }
                            }
                        }
                    }
                }
                stages{
                    stage('dockerBuild2') {
                        agent {
                            docker {
                                image 'docker:dind'
                                args  '-u root -v /tmp:/tmp -v /var/run/docker.sock:/var/run/docker.sock'
                                reuseNode true
                            }
                        }
                        steps {
                            // sh 'docker build -t sairam1007/sample:1.0.2 .'
                            script {
                                dockerImage2 = docker.build registry + ":1.0.3"
                            }
                        }
                    }
                    stage('dockerPush2') {
                        agent {
                            docker {
                                image 'docker:dind'
                                args  '-v /tmp:/tmp -v /var/run/docker.sock:/var/run/docker.sock:rw -u root'
                                reuseNode true
                            }
                        }
                        steps {
                            script {
                                docker.withRegistry( '', registryCredential ) {
                                    dockerImage2.push()
                                }
                            }
                        }
                    }
                }
            }
        }
        // stage('dockerBuild') {
        //      agent {
        //         docker {
        //             image 'docker:dind'
        //             args  '-u root -v /tmp:/tmp -v /var/run/docker.sock:/var/run/docker.sock'
        //             reuseNode true
        //         }
        //     }
        //     steps {
        //         // sh 'docker build -t sairam1007/sample:1.0.1 .'
        //         script {
        //             dockerImage = docker.build registry + ":$BUILD_NUMBER"
        //         }
        //     }
        // }
        // stage('dockerPush') {
        //     agent {
        //         docker {
        //             image 'docker:dind'
        //             args  '-v /tmp:/tmp -v /var/run/docker.sock:/var/run/docker.sock:rw -u root'
        //             reuseNode true
        //         }
        //     }
        //     steps {
        //         script {
        //             docker.withRegistry( '', registryCredential ) {
        //                 dockerImage.push()
        //             }
        //         }
        //     }
        // }
// https://hub.docker.com/repository/docker/sairam1007/sample
// https://registry.hub.docker.com/v1/repositories/sairam1007/sample
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