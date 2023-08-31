pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: "jenkins", url: 'https://github.com/viperiorro/Hillel.Allure'
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
                    // Define a variable 'runArgs' for the Docker run arguments.
                    // '--rm': Remove the container after it exits.
                    // '-v %cd%:/app': Mount the current Jenkins workspace (%cd%) into the '/app' directory in the container.
                    // '-w /app': Set the working directory inside the container to /app.
                    def runArgs = "--rm -v %cd%:/app -w /app"

                    def testCmd = "python -m pytest"

                    // Run the Docker container with the specified arguments and command.
                    bat "docker run ${runArgs} my-app ${testCmd}"
                }
            }
        }
    }
}