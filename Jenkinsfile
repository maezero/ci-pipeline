pipeline {

    agent any
    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '15', 
                    numToKeepStr: '10'
            )
    }

    environment {
        APP_NAME = "webserver"
        APP_VERSION = "1.0.0"
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
        }

        stage('Code Build & Stamp Version') {
                
                    steps {
                        //### Stamp Versiom ####
                        sh "sed -i 's/##VERSION##/"${APP_VERSION}"/g' ./ci-cd-pipeline/app/html/index.html"
                        sh "sed -i 's/##GIT_COMMIT##/"${GIT_COMMIT}"/g' ./ci-cd-pipeline/app/html/index.html"

                        dir('ci-cd-pipeline') {
                            sh 'mv Dockerfile ./app/'                        
                        }
        
                        dir('ci-cd-pipeline/app'){
                            sh "docker build -t zerozang/${APP_NAME}:${APP_VERSION}-${BUILD_NUMBER} ."  
                        }
                        
                    }         
        }

                stage('Push Image') {
                
                    steps {
                        withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                            dir('ci-cd-pipeline/app'){
                                sh "docker logout"
                                sh "docker login -u $USERNAME -p $PASSWORD"
                                sh "docker push zerozang/webserver:1.0.0-${BUILD_NUMBER}"  
                            }
                        }
                    }         
        }
    }   
}
