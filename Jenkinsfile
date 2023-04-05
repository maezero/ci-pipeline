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
                sh 'docker'
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
        }

        stage('Code Build') {
                
                    steps {
                        dir('ci-cd-pipeline') {
                            sh 'mv Dockerfile ./app/'                        
                        }

                        dir('ci-cd-pipeline/app'){
                            sh 'docker build -t ngix-mod:1.0.0-11 .'    
                        }
                    }         
        }
    }   
}
