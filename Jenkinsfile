pipeline {
    agent any
    tools {
        // Use Maven tool configuration; replace 'Maven' with the name you specified in Jenkins' global tool configuration if different
        maven 'Maven'
    }
    stages {
        stage('Build') {
            steps {
                // Execute Maven to clean and package the application
                sh 'mvn clean package'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run Maven with SonarQube analysis
                    sh """
                    mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=SIT753_6.1c \
                    -Dsonar.projectName='SIT753_6.1c' \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.login=sqp_e5d1bed96275d607b085df6c74a56eecb3a0805a
                    """
                }
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                // Execute Maven to run unit and integration tests
                sh 'mvn test'
            }
        }
        stage('Code Analysis') {
            steps {
                // Integrating with SonarQube using Maven again, if separate from initial analysis
                sh 'mvn sonar:sonar'
            }
        }
        stage('Security Scan') {
            steps {
                // Using Maven plugin for security scanning
                sh 'mvn dependency-check:check'
            }
        }
        stage('Deploy to Staging') {
            steps {
                // Deployment command to staging environment
                sh 'aws deploy --stage staging'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                // Execute integration tests in the staging environment
                sh 'selenium tests'  // Ensure this command correctly references your test scripts
            }
        }
        stage('Deploy to Production') {
            steps {
                // Deployment command to production environment
                sh 'aws deploy --stage production'
            }
        }
    }
    post {
        always {
            // Send email notification post-build
            emailext(
                to: 's224078886@deakin.edu.au',
                subject: "Stage Completed - ${currentBuild.fullDisplayName}",
                body: "Pipeline completed."
            )
        }
    }
}