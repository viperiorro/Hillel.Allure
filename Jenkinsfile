pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/viperiorro/Hillel.Allure'
            }
        }

        stage('Prepare Environment') {
            steps {
                script {
                    def appImage = docker.build('my-app')
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    docker.image('my-app').inside {
                        // Run the tests with verbose logging
                        sh 'python -m pytest -ra -vv --alluredir=allure-results"'
                    }
                }
            }
        }
    }

    post {
        always {
            allure includeProperties: false, jdk: '', results: [[path: 'allure-results']]
        }
    }
}