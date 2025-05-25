pipeline {
  agent any

  environment {
    PROD_HOST = "157.245.59.181"  // Ganti sesuai IP server kamu
    DEPLOY_USER = "root"          // User ssh di server kamu (kalau root sesuai request)
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        docker.image('composer:2.5-php8.2').inside('-u root') {
          sh 'rm -f composer.lock'
          sh 'composer install --no-interaction --prefer-dist --optimize-autoloader'
        }
      }
    }

    stage('Testing') {
      steps {
        docker.image('ubuntu').inside('-u root') {
          sh 'echo "Ini adalah test"'
        }
      }
    }

    stage('Deploy to Production') {
      steps {
        docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
          sshagent (credentials: ['github-ssh-naga']) {
            sh '''
              mkdir -p ~/.ssh
              ssh-keyscan -H $PROD_HOST >> ~/.ssh/known_hosts
              rsync -rav --delete ./laravel/ $DEPLOY_USER@$PROD_HOST:/root/prod.kelasdevops.xyz/ --exclude=.env --exclude=storage --exclude=.git
            '''
          }
        }
      }
    }
  }
}
