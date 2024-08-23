
pipeline {
  agent any

  environment {
    APP_SERVER = '54.210.112.109'
    DEPLOY_PATH = '/home/ubuntu/example-app'
  }

  stages {
    stage('Clean Workspace') {
      steps {
        deleteDir()
      }
    }

    stage('Checkout Code') {
      steps {
        git branch: 'main', url: 'https://github.com/Romi293/Laravel_App.git',
	  credentialsId: 'GitHub_Token'
      }
    }

    stage('Build') {
      steps {
        script {
//	  sh 'docker build . -t laravel_image'
//	  sh 'docker push romi293/laravel_app:latest'
        }
      }
    }

    stage('Deploy') {
      steps {
        sshagent(credentials: ['AWS_Laravel']) {
	  sh 'scp -r * ${APP_SERVER}:${DEPLOY_PATH}'
          sh "ssh ${APP_SERVER}'cd ${DEPLOY_PATH}'"
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
