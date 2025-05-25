pipeline {
  agent any
  
  environment {
    PROD_HOST = "157.245.59.181"  // Ganti sesuai host kamu
  }
  
  stages {
    stage('Build') {
      steps {
        // build step
      }
    }
    
    stage('Deploy') {
      steps {
        script {
          docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
            sshagent (credentials: ['github-ssh-naga']) {
              sh 'mkdir -p ~/.ssh'
              sh 'ssh-keyscan -H "$PROD_HOST" >> ~/.ssh/known_hosts'
              sh "rsync -rav --delete ./laravel/ ubuntu@$PROD_HOST:/home/ubuntu/prod.kelasdevops.xyz/ --exclude=.env --exclude=storage --exclude=.git"
            }
          }
        }
      }
    }
  }
}
