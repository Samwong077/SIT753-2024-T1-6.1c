pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    environment {
        PATH = "${env.PATH}:/usr/local/bin" 
    }
    stages {
        stage('Build') {
            steps {
                echo 'Starting Build Stage...'
                sh 'mvn clean package'
                sh 'ls -lah target/'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running Tests...'
                sh 'mvn test'
            }
        }
        stage('Prepare Code Quality Report') {
            steps {
                echo 'Generating JaCoCo Report...'
                sh 'mvn jacoco:report'
            }
        }
        stage('Codacy Analysis') {
            steps {
                echo 'Uploading to Codacy...'
                sh """
                curl -X POST \\
                --data-binary @target/site/jacoco/jacoco.xml \\
                -H "Content-Type: application/xml" \\
                -H "project-token: c1086a37fc8142b98fa6a2bb2681bbf1" \\
                "https://api.codacy.com/2.0/coverage/\${GIT_COMMIT}/xml?provider=manual"
                """
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Conducting Security Scan...'
                sh 'mvn dependency-check:check'
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging...'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Placeholder for integration tests commands'
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to Production...'
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