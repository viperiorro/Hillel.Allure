pipeline {
    agent {
        docker {
            image 'python:3.10.0-alpine'
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
                script {
                    def pythonImage = docker.image('python:3.10')
                    pythonImage.pull()

                    pythonImage.inside {
                        // Install dependencies and run tests
                        bat 'pip install --upgrade pip'
                        bat 'pip install pytest'

                        // If your tests have additional dependencies, install them here:
                        // bat 'pip install -r requirements.txt'

                        // Run the tests
                        bat 'python -m pytest -ra --alluredir=allure-results'
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