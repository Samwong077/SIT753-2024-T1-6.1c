pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'gradle build'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                sh 'gradle test'
            }
        }
        stage('Code Analysis') {
            steps {
                sh 'gradle sonarqube'
            }
        }
        stage('Security Scan') {
            steps {
                sh 'gradle dependencyCheckAnalyze'
            }
        }
        stage('Deploy to Staging') {
            steps {
                sh 'aws deploy --stage staging'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                sh 'selenium tests'
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
            mail to: 's224078886@deakin.edu.au',
                 subject: "Stage Completed - ${currentBuild.fullDisplayName}",
                 body: "Please find the attached logs.",
                 attachments: "logs/*.log"
        }
    }
}