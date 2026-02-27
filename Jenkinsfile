pipeline {
    agent any

    environment {
        POETRY_VIRTUALENVS_CREATE = "true"
        POETRY_NO_INTERACTION = "1"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Poetry') {
            steps {
                sh '''
                curl -sSL https://install.python-poetry.org | python3 -
                export PATH="$HOME/.local/bin:$PATH"
                poetry --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                export PATH="$HOME/.local/bin:$PATH"
                poetry install
                '''
            }
        }

        stage('Static Code Analysis') {
            steps {
                sh '''
                export PATH="$HOME/.local/bin:$PATH"

                poetry run flake8 app.py router models client utils

                poetry run pylint app.py router models client utils --fail-under=7

                poetry run bandit -r app.py router models client utils

                poetry run mypy app.py router models client utils

                poetry run pip-audit
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                export PATH="$HOME/.local/bin:$PATH"
                poetry run pytest --maxfail=1 --disable-warnings --tb=short
                '''
            }
        }
    }

    post {
        success {
            echo 'Build Passed – Quality Gate Satisfied'
        }
        failure {
            echo 'Build Failed – Fix Issues Before Merge'
        }
    }
}
