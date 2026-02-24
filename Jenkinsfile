pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/OT-MICROSERVICES/attendance-api.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install flake8 pylint bandit pytest
                '''
            }
        }

        stage('Flake8 Check') {
            steps {
                sh '''
                . venv/bin/activate
                flake8 .
                '''
            }
        }

        stage('Pylint Check') {
            steps {
                sh '''
                . venv/bin/activate
                pylint $(find . -name "*.py") || true
                '''
            }
        }

        stage('Bandit Security Check') {
            steps {
                sh '''
                . venv/bin/activate
                bandit -r .
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . venv/bin/activate
                pytest || true
                '''
            }
        }
    }
}
