pipeline {
    agent any

    tools {
        // IMPORTANT: Ensure these names exist in your EC2 Jenkins:
        // Manage Jenkins -> Tools -> Maven / JDK
        maven 'local_maven' 
        jdk 'java21'
    }

    environment {
        // If Jenkins & Tomcat are on the SAME EC2 instance, use 'localhost'.
        // If they are different, use the Private IP of the Tomcat instance.
        TOMCAT_URL = 'http://localhost:8080'
        
        APP_CONTEXT_PATH = '/jakarta-app'
        TOMCAT_CREDS = credentials('tomcat-credentials')
        
        // Hardcoded path (Same as before)
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
                echo 'Building on Linux...'
                // CHANGED: 'bat' -> 'sh'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying to ${TOMCAT_URL}..."
                    
                    // CHANGED: 'bat' -> 'sh' and '^' -> '\'
                    sh """
                        curl -v -T "${WAR_FILE}" \
                        "${TOMCAT_URL}/manager/text/deploy?path=${APP_CONTEXT_PATH}&update=true" \
                        --user "${TOMCAT_CREDS_USR}:${TOMCAT_CREDS_PSW}"
                    """
                }
            }
        }
    }
}
