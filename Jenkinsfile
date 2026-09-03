pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
            }
        }
        stage('Install & Test') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install pytest
                    pytest
                '''
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t jenkins-demo-app .'
            }
        }
        stage('Run Container') {
            steps {
                sh 'docker run --rm jenkins-demo-app'
            }
        }
                stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        docker tag jenkins-demo-app $DOCKER_USER/jenkins-demo-app:latest
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push $DOCKER_USER/jenkins-demo-app:latest
                    '''
                }
            }
        }
    }
}
