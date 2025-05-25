node {
    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        // Gunakan image composer resmi dengan PHP 8.1
        docker.image('composer:2.6').inside('-u root') {
            sh '''
            rm -f composer.lock
            composer install --no-interaction --prefer-dist --optimize-autoloader
            '''
        }
    }

    stage('Test') {
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Ini adalah test"'
            // Tambahkan testing command di sini jika perlu
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
