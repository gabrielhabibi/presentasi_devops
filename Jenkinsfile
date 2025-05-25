node {
    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        // Gunakan docker image PHP 8.1 dengan composer
        docker.image('php:8.1-cli').inside('-u root') {
            sh '''
            rm -f composer.lock
            apt-get update && apt-get install -y unzip git curl
            curl -sS https://getcomposer.org/installer | php
            php composer.phar install
            '''
        }
    }

    stage('Test') {
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Ini adalah test"'
            // Bisa kamu tambah testing command disini
        }
    }

    stage('Deploy to Production') {
        docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
            sshagent (credentials: ['ssh-prod']) {
                sh '''
                mkdir -p ~/.ssh
                ssh-keyscan -H "$PROD_HOST" >> ~/.ssh/known_hosts
                rsync -rav --delete ./laravel/ root@$PROD_HOST:/root/prod.kelasdevops.xyz/ --exclude=.env --exclude=storage --exclude=.git
                '''
            }
        }
    }
}
