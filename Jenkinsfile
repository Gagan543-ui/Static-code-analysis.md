pipeline {
    agent any

    environment {
        VENV = "venv"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Setup Virtual Environment & Install Dependencies') {
            steps {
                sh '''
                python3 -m venv $VENV
                . $VENV/bin/activate
                pip install --upgrade pip
                pip install poetry
                poetry install
                '''
            }
        }

        stage('Run Flake8') {
            steps {
                sh '''
                . $VENV/bin/activate
                poetry run flake8 .
                '''
            }
        }

        stage('Run Pylint') {
            steps {
                sh '''
                . $VENV/bin/activate
                poetry run pylint $(find . -name "*.py")
                '''
            }
        }

        stage('Run Bandit (Security Scan)') {
            steps {
                sh '''
                . $VENV/bin/activate
                poetry run bandit -r .
                '''
            }
        }

        stage('Run Pytest') {
            steps {
                sh '''
                . $VENV/bin/activate
                poetry run pytest --maxfail=1 --disable-warnings --tb=short
                '''
            }
        }
    }

    post {
        always {
            echo "CI Pipeline Finished"
        }
        success {
            echo "Build Passed"
        }
        failure {
            echo "Build Failed "
        }
    }
}
