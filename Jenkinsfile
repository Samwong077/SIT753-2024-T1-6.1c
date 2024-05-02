node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def mvn = tool 'Default Maven';
    withSonarQubeEnv() {
      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=SIT753_6.1c -Dsonar.projectName='SIT753_6.1c'"
    }
  }
}
pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    environment {
        SONARQUBE_HOST = 'http://localhost:9000'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/Samwong077/SIT753-2024-T1-6.1c.git', branch: 'main'
            }
        }

        stage('Prepare Workspace') {
            steps {
                script {
                    sh 'ls -la'
                }
            }
        }

        stage('Verify POM') {
            steps {
                script {
                    if (!fileExists('pom.xml')) {
                        error 'pom.xml not found in the workspace. Check SCM settings or project structure.'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                script {
                    sh 'mvn test'
                }
            }
        }

        stage('Code Analysis') {
            steps {
                script {
                    withSonarQubeEnv('Sam') {
                        echo "Analyzing code with SonarQube at ${env.SONARQUBE_HOST}"
                        sh 'mvn sonar:sonar -Dsonar.host.url=${SONARQUBE_HOST}'
                    }
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    echo 'Placeholder for security scan command'
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying to staging server'
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Placeholder for Selenium runner command'
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying to production server'
                }
            }
        }
    }

    post {
        always {
            emailext(
                subject: "Build ${env.JOB_STATUS}: Job ${env.JOB_NAME} Build ${env.BUILD_NUMBER}",
                body: """<p>See detailed results here: <a href='${env.BUILD_URL}'>${env.BUILD_NAME}</a></p>""",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']]
            )
        }
    }
}