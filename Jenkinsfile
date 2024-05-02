pipeline {
    agent any

    tools {
        maven 'Maven'
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
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    sh 'zap-cli quick-scan --self-contained --start-options "-config api.disablekey=true" http://localhost:8083'
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
                    sh 'selenium-side-runner -c "browserName=chrome"'
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
                subject: "Build ${JOB_STATUS}: Job ${JOB_NAME} Build ${BUILD_NUMBER}",
                body: """<p>See detailed results here: <a href='${BUILD_URL}'>${BUILD_NAME}</a></p>""",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']]
            )
        }
    }
}
