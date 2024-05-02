pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Prepare Code Quality Report') {
            steps {
                sh 'mvn jacoco:report'
            }
        }
        stage('Codacy Analysis') {
            steps {
                sh """
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
                sh 'mvn dependency-check:check'
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
            emailext(
                to: 's224078886@deakin.edu.au',
                subject: "Stage Completed - ${currentBuild.fullDisplayName}",
                body: "Pipeline completed."
            )
        }
    }
}