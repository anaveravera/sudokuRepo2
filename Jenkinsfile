pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install') {
      steps {
        sh 'php -v'
        sh 'composer --version || true'
        sh 'composer install'
      }
    }

    stage('Test') {
      steps {
        sh 'vendor/bin/phpunit'
      }
    }

    stage('Deploy Staging') {
      steps {
        echo 'Deploy placeholder: add ssh/scp or platform API call.'
      }
    }
  }
}
