
pipeline {
    agent any

    // Step 1: Inject the tools we configured in the Jenkins UI
    tools {
        maven 'Maven3'
        jdk 'JDK11'
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Pulling the latest code from GitHub..."
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                echo "Running Maven to clean the workspace, compile code, and run tests..."
                // The -B flag runs Maven in 'Batch' mode, which makes the Jenkins logs much cleaner to read.
                // The -DskipTests=false ensures tests are run.
                sh 'mvn -B -DskipTests=false clean package'
            }
            post {
                // If tests fail, Jenkins can still save the test reports so developers can see what broke
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }

    post {
        success {
            echo "Build successful! Archiving the final Artifact..."
            // Grab the finished .jar file and save it to the Jenkins dashboard
            archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
        }
        failure {
            echo "The build failed. Please review the logs."
        }
    }
}
