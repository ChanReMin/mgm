pipeline {
    agent any
    stages {
            stage('Install Node.js') {
                steps {
                    sh 'apt-get update && apt-get install -y nodejs npm'
                }
            }
            stage('List file in root app folder') {
                steps {
                    sh 'ls -la app' // List files in the 'app' directory
                }
            }
            stage('List file in views app folder') {
                steps {
                    sh 'ls -la app/views' // List files in the 'app/views' directory
                }
            }
            stage('Test') {
                steps {
                    dir('app') { // Change directory to 'app' for running test.js
                        sh 'npm install && npm run test'
                    }
                }
            }
    }
}
