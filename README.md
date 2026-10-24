# Grimório

Gerenciador de campanha de D&D 5e: wiki de lore e NPCs com links entre páginas, fichas de personagem e, futuramente, rastreador de combate.

> Em desenvolvimento, Fase 0 (fundação).

## Estrutura

Monorepo: a aplicação web e os serviços de apoio ficam no mesmo repositório, mas rodam como processos separados (monorepo não é monólito). Cada parte cuida das próprias dependências e do próprio `.gitignore`. Na raiz fica só o que vale para todas: `.editorconfig`, `.gitattributes` e, em breve, o Docker Compose.

```
apps/        aplicações voltadas ao usuário (web: Laravel + Inertia + React)
services/    serviços de apoio (dice: rolagem de dados em Go)
docs/adr/    registros de decisões de arquitetura
```

## Como rodar

TODO: entra junto com o Docker Compose ([ADR 0001](docs/adr/0001-execucao-em-containers.md)).
