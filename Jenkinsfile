pipeline {
    agent any
    tools {
        // Use Maven tool configuration; replace 'Maven' with the name you specified in Jenkins' global tool configuration if different
        maven 'Maven'
    }
    stages {
        stage('Build') {
            steps {
                // Replace Gradle build command with Maven
                sh 'mvn clean package'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                // Maven test phase
                sh 'mvn test'
            }
        }
        stage('Code Analysis') {
            steps {
                // Integrating with SonarQube using Maven
                sh 'mvn sonar:sonar'
            }
        }
        stage('Security Scan') {
            steps {
                // If using a Maven plugin for security scanning
                sh 'mvn dependency-check:check'
            }
        }
        stage('Deploy to Staging') {
            steps {
                // Deployment step can remain unchanged unless it specifically requires Gradle
                sh 'aws deploy --stage staging'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                // Assuming integration tests are run through Maven
                sh 'selenium tests'  // Update or keep depending on your integration test setup
            }
        }
        stage('Deploy to Production') {
            steps {
                sh 'aws deploy --stage production'
            }
        }
    }
    post {
        always {
            emailext(
                to: 's224078886@deakin.edu.au',
                subject: "Stage Completed - ${currentBuild.fullDisplayName}",
                body: "Pipeline completed."
            )
        }
    }
}