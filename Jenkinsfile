pipeline {
    agent any

    environment {
        POETRY_NO_INTERACTION = "1"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python 3.11 Environment') {
            steps {
                sh '''
                python3.11 -m venv .venv
                . .venv/bin/activate
                pip install --upgrade pip
                pip install poetry
                poetry --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                . .venv/bin/activate
                poetry install --no-root
                '''
            }
        }

        stage('Static Code Analysis') {
            steps {
                sh '''
                . .venv/bin/activate
                poetry run flake8 app.py router models client utils
                poetry run pylint app.py router models client utils --fail-under=7
                poetry run bandit -r app.py router models client utils -ll
                poetry run mypy app.py router models client utils
                poetry run pip-audit
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . .venv/bin/activate
                poetry run pytest --maxfail=1 --disable-warnings --tb=short
                '''
            }
        }
    }

    post {
        success {
            echo 'Build Passed – Quality Gate Achieved'
        }
        failure {
            echo 'Build Failed – Fix Issues Before Merge'
        }
    }
}
