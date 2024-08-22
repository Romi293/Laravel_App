
pipeline {
  agent any

  environment {
    APP_SERVER = 'http://54.210.112.109'
  }

  stages {
    stage('Checkout Code') {
      steps {
        git url: 'https://github.com/Romi293/Laravel_App.git'
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
          sh
          # scp - r * $ {
            APP_SERVER
          }: $ {
            DEPLOY_PATH
          }
          # ssh $ {
            APP_SERVER
          }
          'cd ${DEPLOY_PATH} && ./deploy-script.sh'
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
