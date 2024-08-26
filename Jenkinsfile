
pipeline {
  agent any

  environment {
    APP_SERVER = '54.210.112.109'
    DEPLOY_PATH = '/home/ubuntu/example-app'
    GIT_BRANCH = 'main'
    REPO_URL = 'https://github.com/Romi293/Laravel_App.git'
    GIT_CREDENTIALS_ID = 'GitHub_Token'
    AWS_CREDENTIALS = 'AWS_Laravel'
    LARAVEL_USER = 'ubuntu'
    LARAVEL_SERVER = 'ec2-54-210-112-109.compute-1.amazonaws.com'
    DOCKER_CONTEXT = 'laravel-remote-context'
  }

  stages {
    stage('Clean Workspace') {
      steps {
        deleteDir()
      }
    }

    stage('Checkout Code') {
      steps {
        git branch: "${GIT_BRANCH}", url: "${REPO_URL}",
	  credentialsId: "${GIT_CREDENTIALS_ID}"
      }
    }

    stage('Build') {
      steps {
        script {
//	  sh "sh 'ssh-keyscan -H "${LARAVEL_SERVER}" >> ~/.ssh/known_hosts'"
	  sh 'docker context use ${DOCKER_CONTEXT}'
	  sh 'docker build -t laravel_image .'
//	  sh 'docker push romi293/laravel_app:latest'
        }
      }
    }

    stage('Deploy') {
      steps {
        sshagent(credentials: ["${AWS_CREDENTIALS}"]) {
	  sh 'ssh-keyscan -H "${LARAVEL_SERVER}" >> ~/.ssh/known_hosts'
	  sh 'ssh -o StrictHostKeyChecking=no "${LARAVEL_USER}"@"${LARAVEL_SERVER}"'
	  sh "rm -rf /home/ubuntu/*"
	  sh 'scp -r example-app/ "${APP_SERVER}":"${DEPLOY_PATH}"'
          sh 'cd "${DEPLOY_PATH}"'
	  sh "./vendor/bin/sail up -d"
//        sh "ssh ${APP_SERVER} 'docker pull romi293/laravel_app:latest'"
//        sh "ssh ${APP_SERVER} 'docker compose up -d'"
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
