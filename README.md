# Opcion 2: Bitbucket + Jira + Jenkins

> Estado actual: este repositorio esta alojado en GitHub.
> El flujo Jira + Jenkins se usa para simular una implementacion tipo Bamboo.

## Flujo

1. Issue Jira: `SUM-1`.
2. Rama: `feature/SUM-1-sudoku-base`.
3. PR en Bitbucket hacia `develop`.
4. Pipeline Jenkins tipo Bamboo por etapas:
	- `1) Checkout`
	- `2) Quality Gate`
	- `3) Install Dependencies`
	- `4) Build + Unit Tests`
	- `5) Package Artifact`
	- `6) Deploy Staging` (en `develop/release`)
	- `7) Smoke Tests (Staging)`
	- `8) Manual Approval`
	- `9) Deploy Production` (en `release/main`)
5. Flujo recomendado: `develop -> release -> main`.

## Activar ejecucion real (Jenkins)

1. Configura Jenkins con este `Jenkinsfile`.
2. Define variables en Jenkins job:
	- `STAGING_DEPLOY_HOOK`
	- `STAGING_HEALTHCHECK_URL`
	- `PRODUCTION_DEPLOY_HOOK`
	- `PRODUCTION_HEALTHCHECK_URL`
3. Para disparar Jenkins desde GitHub configura:
	- Repo Variable: `JENKINS_URL`, `JENKINS_JOB`
	- Repo Secret: `JENKINS_USER`, `JENKINS_API_TOKEN`, `JENKINS_TRIGGER_TOKEN`

Workflow ejecutable en repo:

- `.github/workflows/trigger-jenkins.yml`

## Comandos locales

```bash
composer install
composer test
php -S localhost:8080 -t public
```
