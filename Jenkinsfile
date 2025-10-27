pipeline {
    agent any
    
    environment {
        WAR_FILE_PATH = 'target/my-java-app.war'
        TOMCAT_URL = 'http://localhost:9090/manager/text'
        TOMCAT_CREDENTIALS_ID = 'tomcat-credentials'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // UPDATE THIS LINE: Replace with your actual GitHub URL
                git 'https://github.com/KUNDANPATIL25/Devops7thPractical.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                sh """
                    curl --upload-file ${WAR_FILE_PATH} \
                    --user ${TOMCAT_CREDENTIALS_ID} \
                    ${TOMCAT_URL}/deploy?path=/my-java-app
                """
            }
        }
    }
    
    post {
        success {
            echo 'Application deployed successfully to Tomcat!'
            echo 'Access your app at: http://localhost:8080/my-java-app'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}