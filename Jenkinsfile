pipeline {
    agent any

    tools {
        maven 'local_maven' 
        jdk 'java21'
    }
    environment {
       
        TOMCAT_URL = 'http://192.168.0.98:8080'
        APP_CONTEXT_PATH = '/jakarta-app'
        
    
        TOMCAT_CREDS = credentials('tomcat-deployer')
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
                    
                   
                    def warFile = findFiles(glob: 'target/*.war')[0].path
                    
                 
                    bat """
                        curl -v -T "${warFile}" ^
                        "${TOMCAT_URL}/manager/text/deploy?path=${APP_CONTEXT_PATH}&update=true" ^
                        --user "${TOMCAT_CREDS_USR}:${TOMCAT_CREDS_PSW}"
                    """
                }
            }
        }
    }
}
