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

        stage('Deploy to Kubernetes') {
            environment {
                  KUBECONFIG = '/var/lib/jenkins/.kube/config'
                   }
                   steps {
                       sh 'kubectl apply -f k8s/react_app.yml'
                 }
            }

        stage('Verify Deployment') {
            environment {
                  KUBECONFIG = '/var/lib/jenkins/.kube/config'
                   }
                   steps {
                      sh 'kubectl rollout status deployment/react-app'
                      sh 'kubectl get pods -o wide'
                      sh 'kubectl get svc'
                          }
                   }
    }
}
