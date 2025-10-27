pipeline {
    agent any
    
    environment {
        WAR_FILE_PATH = 'target\\my-java-app.war'
        TOMCAT_URL = 'http://localhost:9090/manager/text'
        TOMCAT_CREDENTIALS_ID = 'tomcat-credentials'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Use checkout scm instead of explicit git to avoid branch issues
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // Use 'bat' for Windows instead of 'sh'
                bat 'mvn clean package'
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                // Use 'bat' and fix the curl command for Windows
                bat "curl --upload-file ${WAR_FILE_PATH} --user %TOMCAT_CREDENTIALS_ID% ${TOMCAT_URL}/deploy?path=/my-java-app"
            }
        }
    }
    
    post {
        success {
            echo 'Application deployed successfully to Tomcat!'
            // Update port to match your Tomcat (9090)
            echo 'Access your app at: http://localhost:9090/my-java-app'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}