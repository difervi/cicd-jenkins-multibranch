pipeline {
    agent any                        

    tools {
        nodejs 'Node18'                
    }                                  

    environment {
        
        APP_PORT  = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        IMAGE_NAME = "${env.BRANCH_NAME == 'main' ? 'nodemain' : 'nodedev'}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm           
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'       
            }
        }

        stage('Test') {
            steps {
                sh 'CI=true npm test'  
            }                                  }

        stage('Build Docker Image') {
            steps {
                                sh "docker build -t ${IMAGE_NAME}:v1.0 ."
            }
        }

        stage('Deploy') {
            steps {

                sh "docker rm -f ${IMAGE_NAME} || true"
                sh "docker run -d --name ${IMAGE_NAME} -p ${APP_PORT}:3000 ${IMAGE_NAME}:v1.0"
            }
        }
    }
}