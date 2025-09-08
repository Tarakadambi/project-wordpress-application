pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "clusterstar/project-wordpress-application"
        DOCKER_TAG = "${env.BUILD_NUMBER}"
        KUBE_CONFIG = credentials('kubeconfig')  // Add your Kubeconfig as a Jenkins secret
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')  // DockerHub credentials
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'app', url: 'https://gitlab.com/tara.kadambi/project-wordpress-application.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $DOCKER_IMAGE:$DOCKER_TAG ."
                    sh "docker tag $DOCKER_IMAGE:$DOCKER_TAG $DOCKER_IMAGE:latest"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                    sh "docker push $DOCKER_IMAGE:$DOCKER_TAG"
                    sh "docker push $DOCKER_IMAGE:latest"
                }
            }
        }

        stage('Helm Lint') {
            steps {
                sh "helm lint wordpress/"
            }
        }

        stage('Deploy to Dev') {
            steps {
                sh "helm upgrade --install wordpress-dev wordpress/ -n dev --set image.tag=$DOCKER_TAG"
                sh "helm test wordpress-dev -n dev"
            }
        }

        stage('Deploy to QA') {
            steps {
                sh "kubectl create namespace qa --dry-run=client -o yaml | kubectl apply -f -"
                sh "helm upgrade --install wordpress-qa wordpress/ -n qa --set image.tag=$DOCKER_TAG"
                sh "helm test wordpress-qa -n qa"
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh "kubectl create namespace staging --dry-run=client -o yaml | kubectl apply -f -"
                sh "helm upgrade --install wordpress-staging wordpress/ -n staging --set image.tag=$DOCKER_TAG"
                sh "helm test wordpress-staging -n staging"
            }
        }

        stage('Manual Approval for Prod') {
            steps {
                input message: 'Deploy to PROD?', ok: 'Deploy'
            }
        }

        stage('Deploy to Prod') {
            when {
                branch 'master'
            }
            steps {
                sh "kubectl create namespace prod --dry-run=client -o yaml | kubectl apply -f -"
                sh "helm upgrade --install wordpress-prod wordpress/ -n prod --set image.tag=$DOCKER_TAG"
                sh "helm test wordpress-prod -n prod"
            }
        }
    }

    post {
        always {
            echo "Pipeline completed"
        }
        success {
            echo "Deployment successful!"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}
