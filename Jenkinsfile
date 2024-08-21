
pipeline {
    agent any

    environment {
        APP_SERVER = 'http://98.81.111.120'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/Romi293/Laravel_App.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['AWS_Laravel']) {
                    sh """
                    # scp -r * ${APP_SERVER}:${DEPLOY_PATH}
                    ssh ${APP_SERVER} 'cd ${DEPLOY_PATH} && ./deploy-script.sh'
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
