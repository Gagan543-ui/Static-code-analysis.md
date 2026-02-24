pipeline {
    agent any

    environment {
        VENV = "venv"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Gagan543-ui/Static-code-analysis.md.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                python3 -m venv $VENV
                . $VENV/bin/activate
                pip install --upgrade pip
                pip install flake8 pylint bandit pytest
                '''
            }
        }

        stage('Run Flake8') {
            steps {
                sh '''
                . $VENV/bin/activate
                flake8 .
                '''
            }
        }

        stage('Run Pylint') {
            steps {
                sh '''
                . $VENV/bin/activate
                pylint $(find . -name "*.py") || true
                '''
            }
        }

        stage('Run Bandit') {
            steps {
                sh '''
                . $VENV/bin/activate
                bandit -r .
                '''
            }
        }

        stage('Run Pytest') {
            steps {
                sh '''
                . $VENV/bin/activate
                pytest || true
                '''
            }
        }
    }
}
