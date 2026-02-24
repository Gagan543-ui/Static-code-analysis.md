pipeline {
    agent any

    environment {
        POETRY_VIRTUALENVS_CREATE = "false"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m pip install --upgrade pip
                pip3 install poetry
                poetry install
                '''
            }
        }

        stage('Run Flake8') {
            steps {
                sh 'poetry run flake8 .'
            }
        }

        stage('Run Pylint') {
            steps {
                sh 'poetry run pylint $(find . -name "*.py")'
            }
        }

        stage('Run Bandit') {
            steps {
                sh 'poetry run bandit -r .'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'poetry run pytest --maxfail=1 --disable-warnings --tb=short'
            }
        }
    }

    post {
        always {
            echo "CI Pipeline Finished"
        }
        success {
            echo "Build Passed "
        }
        failure {
            echo "Build Failed "
        }
    }
}
