pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'ozangulll/devops-v4-try:latest'
        DOCKER_CREDENTIALS_ID = 'dockerhub-creds'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Build with Maven') {
            steps {
                git 'https://github.com/ozangulll/devops-v4'
                sh './mvnw clean package'
            }
        }

        stage('Docker Build an Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh "docker push ${DOCKER_IMAGE}"
            }
        }

        stage('Deploy to Kubernetes - Deployment') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }

        stage('Deploy to Kubernetes - Service') {
            steps {
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }

    }
}
