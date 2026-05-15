# Opcion 2: Bitbucket + Jira + Jenkins

## Flujo

1. Issue Jira: `SUM-1`.
2. Rama: `feature/SUM-1-sudoku-base`.
3. PR en Bitbucket.
4. Jenkins ejecuta `composer install` + `phpunit`.
5. Stage de deploy staging placeholder para SSH/API.

## Comandos locales

```bash
composer install
composer test
php -S localhost:8080 -t public
```
