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
                    dockerImage = docker.build('my-app')
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    def runArgs = "--rm -v %cd%:/app -w /app"
                    def testCmd = "python -m pytest -ra -vv --alluredir=allure-results"
                    bat "docker run ${runArgs} my-app ${testCmd}"
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