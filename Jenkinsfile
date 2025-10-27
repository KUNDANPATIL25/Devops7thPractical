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
                git branch: 'main', url: 'https://github.com/KUNDANPATIL25/Devops7thPractical.git'
                echo 'Checkout stage: Source code checked out'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean compile'
                echo 'Build stage: Application compiled successfully'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
                echo 'Test stage: Tests completed'
            }
        }

        stage('Package') {
            steps {
                bat 'mvn package -DskipTests'
                echo 'Package stage: WAR file created'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Check if WAR file exists
                    def warFile = 'target/jenkins-pipeline-app-1.0-SNAPSHOT.war'
                    if (fileExists(warFile)) {
                        echo "Deploying ${warFile} to Tomcat"
                        // Using single quotes to avoid variable expansion in bat
                        bat """
                            curl -u ${TOMCAT_USER}:${TOMCAT_PASS} --upload-file ${warFile} "${TOMCAT_URL}/deploy?path=/jenkins-app&update=true"
                        """
                    } else {
                        error "WAR file not found: ${warFile}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed'
            // Clean up workspace or archive artifacts if needed
            archiveArtifacts artifacts: 'target/*.war', fingerprint: true
        }
        success {
            echo 'Pipeline succeeded! Application deployed successfully.'
            // You can add notifications here (email, Slack, etc.)
        }
        failure {
            echo 'Pipeline failed! Check the logs for details.'
            // You can add failure notifications here
        }
    }
}