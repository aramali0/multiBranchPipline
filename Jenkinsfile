pipeline {
    agent any

    environment {
        // Define any environment variables here
        MAVEN_HOME = tool 'Maven' // Replace 'Maven' with your Maven tool installation name in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository for the specific branch
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Conditional build logic based on branch name
                    if (env.BRANCH_NAME == 'main') {
                        echo 'Building master branch...'
                        // Use Maven to build
                        sh "${MAVEN_HOME}/bin/mvn clean install"
                    } else if (env.BRANCH_NAME == 'develop') {
                        echo 'Building develop branch...'
                        // Use Maven to build with different profile
                        sh "${MAVEN_HOME}/bin/mvn clean install -Pdevelopment"
                    } else {
                        echo "Building branch ${env.BRANCH_NAME}..."
                        sh "${MAVEN_HOME}/bin/mvn clean install -Pfeature"
                    }
                }
            }
        }

        stage('Test') {
            steps {
                // Run tests after build
                sh "${MAVEN_HOME}/bin/mvn test"
            }
        }

        stage('Post-Build Actions') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'master') {
                        echo 'Deploying to production...'
                        // Add deployment script for master
                    } else if (env.BRANCH_NAME == 'develop') {
                        echo 'Deploying to staging...'
                        // Add deployment script for develop
                    } else {
                        echo "Skipping deployment for branch ${env.BRANCH_NAME}"
                    }
                }
            }
        }
    }

    post {
        always {
            // Archive test results
            junit '**/target/surefire-reports/*.xml'

            // Archive artifacts
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true

            // Cleanup workspace
            cleanWs()
        }

        failure {
            echo 'Build failed!'
            // Optionally send notifications on failure
        }

        success {
            echo 'Build succeeded!'
            // Optionally send notifications on success
        }
    }
}
