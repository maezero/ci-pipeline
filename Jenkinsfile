pipeline {

    agent any

    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '15', 
                    numToKeepStr: '10'
            )
    }

    environment {
        APP_NAME = "TEST_APP"
        APP_ENV  = "DEV"
    }

    stages {
        
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh """
                echo "Cleaned Up Workspace for ${APP_NAME}"
                """
            }
        }

        stage('Code Checkout') {
                steps {
                        checkout([
                            $class: 'GitSCM', 
                            branches: [[name: '*/main']], 
                            userRemoteConfigs: [[url: 'https://github.com/maezero/challenges.git']]
                        ])
                }

                steps {
                sh 'docker'
            }           
        }

//        stage('Code Build') {
//            agent {
                // Equivalent to "docker build -f Dockerfile.build --build-arg version=1.0.2 ./build/
//                dockerfile {
//                    filename './ci-cd-pipeline/app/Dockerfile'
//                    dir './ci-cd-pipeline/app/'
//                    label 'my-image'
//                    additionalBuildArgs  '--build-arg version=1.0.2'
//                    args '-v /tmp:/tmp'
//                }
//            }
            
//        }
    }   
}
