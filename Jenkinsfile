node {
  checkout scm

  stage("Build"){
    docker.image('composer:2').inside('-u root') {
      sh 'rm -f composer.lock'
      sh 'composer install --no-interaction --prefer-dist --optimize-autoloader'
    }
  }

  stage("Test") {
    docker.image('ubuntu').inside('-u root') {
      sh 'echo "Ini adalah test"'
    }
  }

  stage("Deploy") {
    withEnv(["PROD_HOST=157.245.59.181"]) {
      docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
        sshagent (credentials: ['github-ssh-naga']) {
          sh '''
            mkdir -p ~/.ssh
            ssh-keyscan -H "$PROD_HOST" >> ~/.ssh/known_hosts
            rsync -rav --delete ./laravel/ root@$PROD_HOST:/root/prod.kelasdevops.xyz/ \
              --exclude=.env --exclude=storage --exclude=.git
          '''
        }
      }
    }
  }
}
