pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    environment {
        // Define SonarQube server and authentication token
        SONARQUBE_HOST = 'http://localhost:9000'
        SONARQUBE_AUTH_TOKEN = 'sqp_a7e0632a87ec26bbf0239fd74870d1d9c09c2852'
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the Git repository
                git url: 'https://github.com/Samwong077/SIT753-2024-T1-6.1c.git', branch: 'main'
            }
        }

        stage('Prepare Workspace') {
            steps {
                // List files in the workspace for debugging
                script {
                    sh 'ls -la'
                }
            }
        }

        stage('Verify POM') {
            steps {
                // Verify that the Maven pom.xml file exists
                script {
                    if (!fileExists('pom.xml')) {
                        error 'pom.xml not found in the workspace. Check SCM settings or project structure.'
                    }
                }
            }
        }

        stage('Check SonarQube Connection') {
            steps {
                // Check if SonarQube server is reachable
                script {
                    sh 'curl ${env.SONARQUBE_HOST}'
                }
            }
        }

        stage('Build') {
            steps {
                // Build the project using Maven
                script {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                // Run unit and integration tests using Maven
                script {
                    sh 'mvn test'
                }
            }
        }

        stage('Code Analysis') {
            steps {
                // Analyze code with SonarQube
                script {
                    withSonarQubeEnv('SonarQube') {
                        echo "Analyzing code with SonarQube at ${env.SONARQUBE_HOST}"
                        sh 'mvn sonar:sonar -Dsonar.projectKey=SIT753_6.1c -Dsonar.host.url=${env.SONARQUBE_HOST} -Dsonar.login=${env.SONARQUBE_AUTH_TOKEN}'
                    }
                }
            }
        }

        stage('Security Scan') {
            steps {
                // Placeholder for security scan command
                script {
                    echo 'Placeholder for security scan command'
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                // Placeholder for deployment to staging server
                script {
                    echo 'Deploying to staging server'
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                // Placeholder for Selenium runner command
                script {
                    echo 'Placeholder for Selenium runner command'
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                // Placeholder for deployment to production server
                script {
                    echo 'Deploying to production server'
                }
            }
        }
    }

    post {
        always {
            // Send email notifications
            emailext(
                subject: "Build ${env.JOB_STATUS}: Job ${env.JOB_NAME} Build ${env.BUILD_NUMBER}",
                body: """<p>See detailed results here: <a href='${env.BUILD_URL}'>${env.BUILD_NAME}</a></p>""",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']]
            )
        }
    }
}