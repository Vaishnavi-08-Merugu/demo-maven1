pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Pull code from GitHub automatically
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing npm dependencies...'
                bat 'npm install'      // for Windows Jenkins agent
                // use sh 'npm install' if using Linux
            }
        }

        stage('Build and Test') {
            steps {
                echo 'Building and testing the project...'
                bat 'npm run build'
                bat 'npm run test'
            }
        }

        stage('Archive Artifact') {
            steps {
                echo 'Archiving build output...'
                // Create a zip file of the project
                bat 'tar -a -c -f build-artifact.zip index.js package.json'
                archiveArtifacts artifacts: 'build-artifact.zip', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed! Please check logs.'
        }
    }
}
