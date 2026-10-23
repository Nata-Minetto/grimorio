# 0001. Execução em containers

- Status: Aceito
- Data: 2026-10-23

## Contexto
O projeto roda com Laravel 12 (PHP 8.4), Postgres 17, Node e Go, e depende dessas versões específicas para funcionar corretamente.

Por isso, é necessário garantir que essas versões sejam as mesmas em todos os ambientes: na máquina de quem desenvolve ou roda o projeto, no CI (GitHub Actions) e, futuramente, no servidor de produção, ainda não definido.

Sem essa garantia, pode haver conflitos de versões ou até a falta de componentes para que a aplicação execute corretamente. Isso exigiria tempo para instalar individualmente as versões corretas.

## Decisão
Optamos pelo uso de containers com Docker Compose para garantir a compatibilidade de versões e reduzir a preparação do ambiente a poucos comandos no terminal. A única dependência necessária na máquina passa a ser o Docker.

## Alternativas consideradas
- **Instalação direta na máquina**: mais simples para quem já tem o ambiente e sem o consumo de memória e processamento do Docker. Descartada porque não garante as mesmas versões entre os ambientes (ver Contexto).

- **Laravel Sail**: compose oficial do time do Laravel, com documentação, atalhos de comandos e nenhuma configuração inicial necessária. Descartada porque o repositório contém, além do Laravel, um serviço em Go, o que exigiria customizar os arquivos da ferramenta com esforço parecido ao de escrever um compose próprio, mas preso às convenções do Sail.

## Consequências
**Positivas**

- Ambiente reproduzível: qualquer máquina com Docker roda o projeto com as mesmas versões.
- Início rápido: `git clone` + `docker compose up` e o projeto está rodando.
- Isolamento: o projeto não interfere em outros da máquina, e vice-versa.
- Não amarra o modelo de distribuição: serve tanto para self-hosted quanto para hospedagem.

**Negativas**

- Docker vira pré-requisito: quem não tem ou não conhece Docker encontra uma barreira de entrada.
- Consumo de recursos: o Docker Desktop no Windows e no Mac usa RAM e CPU mesmo parado.
- Manutenção própria: o compose e os Dockerfiles são responsabilidade do projeto.
- Primeira execução lenta: baixar as imagens e montar os containers na primeira vez leva tempo.
