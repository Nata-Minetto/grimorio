# Grimório

Gerenciador de campanha de D&D 5e: wiki de lore e NPCs com links entre páginas, fichas de personagem e, futuramente, rastreador de combate.

> Em desenvolvimento, Fase 0 (fundação).

## Estrutura

Monorepo: a aplicação web e os serviços de apoio ficam no mesmo repositório, mas rodam como processos separados (não é monólito). Cada parte cuida das próprias dependências e do próprio `.gitignore`. Na raiz fica só o que vale para todas: `.editorconfig`, `.gitattributes`, o Docker Compose (`compose.yaml`) e o CI (`.github/`).

```
apps/        aplicações voltadas ao usuário (web: Laravel + Inertia + React)
services/    serviços de apoio (dice: rolagem de dados em Go)
docs/adr/    registros de decisões de arquitetura
```

## Como o projeto é desenvolvido

O projeto é conduzido por mim, no papel de desenvolvedor e de tech lead: defino escopo, stack e arquitetura, reviso cada mudança e registro as decisões nos [ADRs](docs/adr/). A implementação tem o [Claude Code](https://claude.com/claude-code) como par de programação, e por isso os commits trazem o Claude como coautor.

## Como rodar

A única dependência é o Docker ([ADR 0001](docs/adr/0001-execucao-em-containers.md)). PHP, Node e Postgres rodam em containers.

Primeira vez:

```bash
cp apps/web/.env.example apps/web/.env
docker compose build
docker compose run --rm --no-deps app composer install
docker compose run --rm --no-deps vite npm install
docker compose run --rm --no-deps app php artisan key:generate
docker compose up -d
docker compose exec app php artisan migrate
```

Depois, basta `docker compose up -d`. A aplicação fica em http://localhost:8000.

Os containers rodam com o UID 1000 por padrão. Se o seu for outro, exporte `HOST_UID` e `HOST_GID` antes de subir.

### Qualidade

```bash
docker compose exec app composer lint:check   # Pint
docker compose exec app composer analyse      # Larastan
docker compose exec app composer test         # Pest
docker compose run --rm --no-deps vite sh -c "npx eslint . && npx tsc --noEmit"
```
