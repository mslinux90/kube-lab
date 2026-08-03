pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
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

        stage('Build Docker Image') {
            steps {
                dir('frontend') {
                    sh 'docker build -t react-k8s:${BUILD_NUMBER} .'
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh '''
                  docker tag react-k8s:${BUILD_NUMBER} \
                  mslinux90/react-k8s:${BUILD_NUMBER}
                  '''
            }
        }

        stage('Push Docker Image') {
            steps {
                 withCredentials([usernamePassword(
                 credentialsId: 'dockerhub-creds',
                 usernameVariable: 'DOCKER_USER',
                 passwordVariable: 'DOCKER_PASS'
        )]) {
                 sh '''
                 echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                 docker push mslinux90/react-k8s:${BUILD_NUMBER}
                 docker logout
                 '''
                }
            }
        }

        stage('Deploy using Ansible') {
            steps {
               dir("${WORKSPACE}") {  
                sh '''
                ansible-playbook \
                -i ansible/inventory \
                ansible/deploy.yml \
                -e image_tag=${BUILD_NUMBER}
                '''
            }
        }
    }
}
 
        post {

        success {
            echo "Deployment Successful"
        }

        failure {
            echo "Deployment Failed"
        }

        always {
            sh 'docker images | grep react-k8s || true'
        }
    }
}

