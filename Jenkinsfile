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
	  credentialsId: 'GitHub_Token'
      }
    }

    stage('Install Docker') {
        steps {
            script {
                sh '''
                apt-get update
                apt-get install -y apt-transport-https ca-certificates curl software-properties-common
                curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
                add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
                apt-get update
                apt-get install -y docker-ce
                '''
            }
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
