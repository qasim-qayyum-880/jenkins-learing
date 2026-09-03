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
    }
}
