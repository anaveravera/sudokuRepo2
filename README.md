# Opcion 2: Bitbucket + Jira + Jenkins

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

## Comandos locales

```bash
composer install
composer test
php -S localhost:8080 -t public
```
