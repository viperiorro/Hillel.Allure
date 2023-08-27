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
                    docker.withRegistry('', 'docker.io') {
                        dockerImage.run('--rm -v %cd%:/app -w /app',
                            'bash -c "python -m pytest -ra -vv --alluredir=allure-results || echo Pytest failed"')
                    }
                }
            }
        }
    }

    post {
        always {
            allure commandline: 'allure', includeProperties: false, jdk: '', results: [[path: 'allure-results']]
        }
    }
}