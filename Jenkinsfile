pipeline {
    agent any

    environment {
        TOMCAT_URL = 'http://localhost:9090/manager/text'
        TOMCAT_USER = 'admin'
        TOMCAT_PASS = 'password'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from version control
                // Replace with your actual repository URL
                git 'https://github.com/KUNDANPATIL25/Devops7thPractical.git'
                echo 'Checkout stage: Source code checked out'
            }
        }

        stage('Build') {
            steps {
                // Use Maven to build the application
                bat 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                // Run tests using Maven
                bat 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy the application to the Tomcat server using Manager API
                bat '''
                curl -u %TOMCAT_USER%:%TOMCAT_PASS% --upload-file target/jenkins-pipeline-app-1.0-SNAPSHOT.war "%TOMCAT_URL%/deploy?path=/jenkins-app&update=true"
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
