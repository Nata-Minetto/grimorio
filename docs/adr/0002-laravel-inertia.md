# 0002. Laravel e Inertia com React

- Status: Aceito
- Data: 2026-10-24

## Contexto
A aplicação web é onde o mestre gerencia a campanha: wiki de lore e NPCs, fichas de personagem e, futuramente, o combate. Uma pessoa desenvolve e mantém o projeto.

A interface precisa ser interativa em vários pontos: editor de markdown com preview, autocomplete de `[[links]]` entre páginas e fichas que recalculam valores enquanto são editadas. Ao mesmo tempo, a aplicação não tem nenhum consumidor além da própria interface, então manter uma API separada só para o frontend seria custo sem retorno.

O projeto também é portfólio. O objetivo é demonstrar Laravel no backend e React com TypeScript no frontend, além da integração com serviços próprios, como o serviço de dados em Go (ADR 0004).

## Decisão
Optamos por Laravel 12 (PHP 8.4) com Inertia 2, React 19 e TypeScript, partindo do starter kit React oficial (`laravel/react-starter-kit` v1.0.1), que já traz Tailwind e shadcn/ui.

O Laravel continua responsável por rotas, controllers, validação, autenticação e autorização. O Inertia entrega os dados de cada página como props, e o React só renderiza. Assim, não há necessidade de uma API intermediária.

## Alternativas consideradas
- **API REST + SPA separada**: o backend expõe uma API e o React vira uma aplicação independente. Descartada porque duplica a estrutura (duas aplicações, autenticação por token, contrato e versionamento de API) sem ter outro consumidor além da própria interface.

- **Blade puro**: páginas renderizadas no servidor, com JavaScript pontual. Descartada porque a interatividade do editor e das fichas exigiria muito JavaScript avulso, sem estrutura.

- **Laravel 13 com a versão atual do starter kit**: mesma stack, versões mais novas. Descartada para seguir a versão já definida no planejamento. O upgrade será reavaliado depois do MVP.

## Consequências
**Positivas**

- Uma aplicação só: rotas, autenticação por sessão e autorização ficam no Laravel, sem duplicar regras no frontend.
- Início rápido: o starter kit entrega login, registro, redefinição de senha e layout prontos.
- Tipagem no frontend com TypeScript, verificada no CI.
- Stack padrão do ecossistema Laravel, com documentação oficial e comunidade ativa.

**Negativas**

- Backend e frontend acoplados: se surgir um consumidor externo (um app mobile ou uma integração), será preciso criar uma API.
- O kit v1.0.1 é uma versão congelada: veio com erros de tipo que precisaram ser corrigidos no projeto e não receberá correções.
- Sem upgrade de versão até o fim do MVP, salvo necessidade forte. Com isso, o projeto fica para trás da versão atual do framework nesse período.
- O kit traz código que não é do domínio do projeto (página de boas-vindas, telas de configuração), que precisa ser adaptado ou removido.
