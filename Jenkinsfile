pipeline {
    agent {
        docker {
            image 'python:3.10.0-alpine'
            args '-v /var/jenkins_home/workspace/allure:/app'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/viperiorro/Hillel.Allure'
            }
        }

        stage('Prepare Environment') {
            steps {
                sh 'python -m pip install --upgrade pip'
                sh 'python -m pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'cd /app'
                sh 'python -m pytest -ra --alluredir=allure-results'
            }
        }
    }

    post {
        always {
            allure includeProperties: false, jdk: '', results: [[path: 'allure-results']]
        }
    }

}