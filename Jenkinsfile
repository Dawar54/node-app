pipeline {
agent any

environment {
    DOCKER_IMAGE = "dawr1234/node-app"
    TAG = "${BUILD_NUMBER}"
}

stages {

    stage('Clone Repo') {
        steps {
            checkout scm
        }
    }

    stage('Build Docker Image') {
        steps {
            sh 'docker build -t $DOCKER_IMAGE:$TAG .'
        }
    }

    stage('Push Image') {
        steps {
            withCredentials([usernamePassword(
                credentialsId: 'dockerhub-creds',
                usernameVariable: 'USER',
                passwordVariable: 'PASS'
            )]) {
                sh '''
                echo $PASS | docker login -u $USER --password-stdin
                docker push $DOCKER_IMAGE:$TAG
                '''
            }
        }
    }
}

}
