node {
  checkout scm

  stage("Build") {
    dir('laravel') {
      docker.image('composer:2').inside('-u root') {
        sh 'rm -f composer.lock'
        sh 'composer install --no-interaction --prefer-dist --optimize-autoloader'
      }
    }
  }

  stage("Testing") {
    docker.image('ubuntu').inside('-u root') {
      sh 'echo "Ini adalah test"'
    }
  }

  stage("Deploy to Production") {
    docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
      sshagent (credentials: ['github-ssh-naga']) {
        sh 'mkdir -p ~/.ssh'
        sh 'ssh-keyscan -H "$PROD_HOST" >> ~/.ssh/known_hosts'
        sh '''
          rsync -rav --delete ./laravel/ \
          root@$PROD_HOST:/root/prod.kelasdevops.xyz/ \
          --exclude=.env --exclude=storage --exclude=.git
        '''
      }
    }
  }
}
