pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    environment {
        PATH = "${env.PATH};C:\\Program Files\\Amazon\\AWSCLIV2"
    }
    stages {
        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                bat 'mvn test'
            }
        }
        stage('Prepare Code Quality Report') {
            steps {
                bat 'mvn jacoco:report'
            }
        }
        stage('Codacy Analysis') {
            steps {
                bat """
                curl -X POST \\
                --data-binary @\${WORKSPACE}/target/site/jacoco/jacoco.xml \\
                -H "Content-Type: application/xml" \\
                -H "project-token: c1086a37fc8142b98fa6a2bb2681bbf1" \\
                "https://api.codacy.com/2.0/coverage/\${GIT_COMMIT}/xml?provider=manual"
                """
            }
        }
        stage('Security Scan') {
            steps {
                bat 'mvn dependency-check:check'
            }
        }
        stage('Deploy to Staging') {
            steps {
                bat 'echo %PATH%'
                bat 'where aws'
                bat 'aws deploy --stage staging'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                bat 'selenium tests'
            }
        }
        stage('Deploy to Production') {
            steps {
                bat 'aws deploy --stage production'
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