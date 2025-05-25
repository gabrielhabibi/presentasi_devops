pipeline {
  agent any

  environment {
    PROD_HOST = "157.245.59.181"  // Ganti dengan IP server kamu
    DEPLOY_USER = "ubuntu"         // Ganti dengan user ssh di server kamu
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        docker.image('composer:2').inside('-u root') {
          sh 'rm -f composer.lock'
          sh 'composer install --no-interaction --prefer-dist --optimize-autoloader'
        }
      }
    }

    stage('Test') {
      steps {
        docker.image('ubuntu').inside('-u root') {
          sh 'echo "Ini adalah test"'
        }
      }
    }

    stage('Deploy') {
      steps {
        docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
          sshagent (credentials: ['github-ssh-naga']) {
            sh '''
              mkdir -p ~/.ssh
              ssh-keyscan -H $PROD_HOST >> ~/.ssh/known_hosts
              rsync -rav --delete ./laravel/ $DEPLOY_USER@$PROD_HOST:/home/ubuntu/prod.kelasdevops.xyz/ --exclude=.env --exclude=storage --exclude=.git
            '''
          }
        }
      }
    }
  }
}
