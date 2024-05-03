pipeline {
    agent any
    tools {
        maven 'Maven' // Make sure this tool configuration name matches your Jenkins configuration
    }
    environment {
        // Ensure the Docker and Maven are correctly configured in the system path
        PATH = "${env.PATH}:/usr/local/bin:/usr/bin:/bin"
    }
    stages {
        stage('Preparation') {
            steps {
                echo 'Preparing the environment...'
                sh 'echo Checking Docker...'
                sh 'docker --version' // This will help verify Docker is accessible
                sh 'echo Checking Maven...'
                sh 'mvn --version' // This confirms Maven is set up correctly
                sh 'pwd'
                sh 'ls -la' // Lists files in the current directory to debug SCM checkout issues
            }
        }
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
                sh '''
                curl -X POST \
                --data-binary @target/site/jacoco/jacoco.xml \
                -H "Content-Type: application/xml" \
                -H "project-token: c1086a37fc8142b98fa6a2bb2681bbf1" \
                "https://api.codacy.com/2.0/coverage/${GIT_COMMIT}/xml?provider=manual"
                '''
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
                sh 'docker build -t myapp:staging .'
                sh 'docker run -d --name myapp-staging myapp:staging'
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
                sh 'docker build -t myapp:latest .'
                sh 'docker run -d --name myapp-production myapp:latest'
            }
        }
    }
    post {
        always {
            emailext(
                to: 's224078886@deakin.edu.au',
                subject: "Stage Completed - ${currentBuild.fullDisplayName}",
                body: "Pipeline completed successfully."
            )
        }
    }
}