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
        stage('Build') {
            steps {
                git 'https://github.com/ozangulll/devops-v4'
                sh './mvnw clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push ${DOCKER_IMAGE}
                    """
                }
            }
        }
    }
}
