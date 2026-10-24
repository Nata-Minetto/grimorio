# 0003. PostgreSQL

- Status: Aceito
- Data: 2026-10-24

## Contexto
Os dados do projeto são majoritariamente relacionais: campanhas têm entradas de wiki, as entradas se ligam umas às outras por `[[links]]` e as fichas pertencem a campanhas e podem estar vinculadas a NPCs, criaturas ou personagens jogáveis.

Algumas partes, porém, são flexíveis. A ficha de D&D 5e tem inventário, magias e traços com formatos variados, que não cabem bem em colunas fixas. A wiki também precisa de busca por texto, em português.

O starter kit vem configurado com SQLite.

## Decisão
Optamos pelo PostgreSQL 17, rodando no container `db` do Docker Compose (ver ADR 0001).

Os testes automatizados continuam usando SQLite em memória, que é mais rápido e não depende do container do banco.

## Alternativas consideradas
- **SQLite**: o padrão do starter kit, um arquivo local sem servidor. Descartada porque a busca textual é limitada e o banco suporta mal escritas concorrentes, o que pesa quando os jogadores entrarem (Fase 5).

- **MySQL / MariaDB**: o banco mais comum em hospedagem PHP. Descartada porque o suporte a JSON e a busca textual são mais fracos que o JSONB e o full-text do PostgreSQL, que tem dicionário para português.

## Consequências
**Positivas**

- JSONB indexável para as partes flexíveis da ficha, sem abrir mão das colunas relacionais.
- Busca textual nativa para a wiki, sem serviço extra (Elasticsearch, Meilisearch).
- Integridade forte: chaves estrangeiras e constraints validadas pelo banco.

**Negativas**

- Mais um container rodando no ambiente de desenvolvimento.
- Bancos diferentes entre os testes (SQLite) e o desenvolvimento (PostgreSQL). Funcionalidades específicas do PostgreSQL, como JSONB e full-text, vão precisar de testes rodando no PostgreSQL quando forem implementadas.
- Hospedagens PHP mais baratas costumam oferecer só MySQL, o que restringe as opções de deploy.
