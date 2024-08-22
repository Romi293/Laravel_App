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

    stage('Install Docker and Docker Compose') {
      steps {
        script {
          sh '''
          sudo apt-get update
          sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
          curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
          sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
          sudo apt-get update
          sudo apt-get install -y docker-ce
	  sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d '"' -f 4)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
          sudo chmod +x /usr/local/bin/docker-compose
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
