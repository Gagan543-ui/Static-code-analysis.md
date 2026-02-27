pipeline {
    agent any

    environment {
        PATH = "$HOME/.local/bin:$PATH"
        POETRY_NO_INTERACTION = "1"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                poetry install
                '''
            }
        }

        stage('Static Code Analysis') {
            steps {
                sh '''
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
