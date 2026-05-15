pipeline {
  agent any

  options {
    timestamps()
  }

  stages {
    stage('1) Checkout') {
      steps {
        checkout scm
      }
    }

    stage('2) Quality Gate') {
      steps {
        sh 'php -l public/index.php'
      }
    }

    stage('3) Install Dependencies') {
      steps {
        sh 'php -v'
        sh 'composer --version || true'
        sh 'composer install --no-interaction --prefer-dist'
      }
    }

    stage('4) Build + Unit Tests') {
      steps {
        sh 'vendor/bin/phpunit'
      }
    }

    stage('5) Package Artifact') {
      steps {
        sh 'tar -czf sudoku-artifact.tar.gz public src tests composer.json composer.lock phpunit.xml'
        archiveArtifacts artifacts: 'sudoku-artifact.tar.gz', fingerprint: true
      }
    }

    stage('6) Deploy Staging') {
      when {
        anyOf {
          branch 'develop'
          branch 'release'
        }
      }
      steps {
        echo 'Deploy STAGING (placeholder): Render/VPS/SSH/API.'
      }
    }

    stage('7) Smoke Tests (Staging)') {
      when {
        anyOf {
          branch 'develop'
          branch 'release'
        }
      }
      steps {
        echo 'Smoke test STAGING (placeholder): curl health endpoint.'
      }
    }

    stage('8) Manual Approval') {
      when {
        anyOf {
          branch 'release'
          branch 'main'
        }
      }
      steps {
        input message: 'Aprobar despliegue a PRODUCCION?', ok: 'Desplegar'
      }
    }

    stage('9) Deploy Production') {
      when {
        anyOf {
          branch 'release'
          branch 'main'
        }
      }
      steps {
        echo 'Deploy PRODUCCION (placeholder): Render/VPS/SSH/API.'
      }
    }
  }
}
