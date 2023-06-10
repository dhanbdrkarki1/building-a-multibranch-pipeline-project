pipeline {
    agent {
        docker {
            image 'node:lts-alpine'
            args '-p 3000:3000 -p 5000:5000'
        }
    }
    environment{
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'echo "Hello world!"'
                sh 'npm install'
            }
        }
        stage('Test'){
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver for development'){
            when {
                branch 'development'
            }
            steps{
                sh './jenkins/scripts/deliver-for-development.sh'
                input message: 'Finished using the website? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }

        stage('Deliver for production'){
            when {
                branch 'production'
            }
            steps{
                sh './jenkins/scripts/deliver-for-production.sh'
                input message: 'Finished using the website? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
