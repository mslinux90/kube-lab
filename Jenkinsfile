pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Node Modules') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build React') {
            steps {
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }

    }
}
