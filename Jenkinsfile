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
        sh '''
          if [ -z "$STAGING_DEPLOY_HOOK" ]; then
            echo "STAGING_DEPLOY_HOOK no configurado. Saltando deploy real."
            exit 0
          fi
          curl -fsS -X POST "$STAGING_DEPLOY_HOOK"
        '''
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
        sh '''
          if [ -z "$STAGING_HEALTHCHECK_URL" ]; then
            echo "STAGING_HEALTHCHECK_URL no configurado. Saltando smoke test."
            exit 0
          fi
          curl -fsS "$STAGING_HEALTHCHECK_URL/health.php"
        '''
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
        sh '''
          if [ -z "$PRODUCTION_DEPLOY_HOOK" ]; then
            echo "PRODUCTION_DEPLOY_HOOK no configurado. Saltando deploy real."
            exit 0
          fi
          curl -fsS -X POST "$PRODUCTION_DEPLOY_HOOK"

          if [ -z "$PRODUCTION_HEALTHCHECK_URL" ]; then
            echo "PRODUCTION_HEALTHCHECK_URL no configurado. Saltando smoke test prod."
            exit 0
          fi
          curl -fsS "$PRODUCTION_HEALTHCHECK_URL/health.php"
        '''
      }
    }
  }
}
