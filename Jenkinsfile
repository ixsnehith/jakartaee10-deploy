pipeline {
    agent any

    tools {
        maven 'local_maven' 
        jdk 'java21'
    }

    environment {
        TOMCAT_URL = 'http://localhost:8080/'
        APP_CONTEXT_PATH = '/jakarta-app'
        TOMCAT_CREDS = credentials('tomcat-credentials')
        
        WAR_FILE = 'target/jakartaee10-starter-boilerplate.war'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building on Windows...'
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying to ${TOMCAT_URL}..."
                    
                    bat """
                        curl -v -T "${WAR_FILE}" ^
                        "${TOMCAT_URL}/manager/text/deploy?path=${APP_CONTEXT_PATH}&update=true" ^
                        --user "${TOMCAT_CREDS_USR}:${TOMCAT_CREDS_PSW}"
                    """
                }
            }
        }
    }
}
