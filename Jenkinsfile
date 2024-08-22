
pipeline {
  agent any

  environment {
    APP_SERVER = 'http://54.210.112.109'
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
	    credentialsId: 'GITHUB_TOKEN'
      }
    }

    stage('Build') {
      steps {
        script {
	  sh 'docker build . -t laravel_image'
	  sh 'docker push romi293/laravel_app:latest'
        }
      }
    }

    stage('Deploy') {
      steps {
        sshagent(credentials: ['AWS_Laravel']) {
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
