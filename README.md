# palte-hub

Hub do Palte, assistente de IA que coleta documentos de clientes de contabilidades pequenas via WhatsApp, classifica automaticamente e cobra pendências.

## Personas

- **Marina** (contadora): usa o hub para acompanhar documentos pendentes, configurar cobranças e visualizar o status de cada cliente.
- **Roberto** (cliente da contabilidade): interage exclusivamente pelo WhatsApp, enviando documentos e recebendo lembretes automáticos.

## Stack

- **API:** FastAPI + SQLAlchemy 2.0 + Alembic (Python)
- **Worker:** Celery — OCR, classificação de documentos e disparo de lembretes
- **Front:** Next.js + TypeScript
- **Banco de dados:** Postgres com Row-Level Security
- **Cache / fila:** Redis
- **WhatsApp:** Meta Cloud API

## Princípios de arquitetura

- Multi-tenant com schema compartilhado + `tenant_id` + RLS desde o início
- Webhook responde em menos de 2 segundos; processamento pesado sempre assíncrono no worker
- Validação de assinatura em toda requisição do webhook
- `api` e `worker` compartilham models, migrations e domínio via `/shared`
- Conformidade LGPD no armazenamento de documentos de terceiros

## Estrutura de pastas

```
palte-hub/
├── web/      Next.js + TypeScript — front do hub do contador (app.<dominio>)
├── api/      FastAPI + SQLAlchemy 2.0 + Alembic — backend e webhook WhatsApp (api.<dominio>)
├── worker/   Celery — OCR, classificação de documentos e disparo de lembretes
├── shared/   Models e camada de domínio Python compartilhados entre api e worker
├── infra/    Docker Compose de desenvolvimento (Postgres, Redis)
└── docs/     DER, casos de uso, regras de negócio, contrato de API, templates WhatsApp
```

## Fluxo de branches

- `feature/*` e `fix/*` saem de `dev`
- PR de `feature/*` ou `fix/*` para `dev`
- Release via PR de `dev` para `main`
- `main` está protegida: 1 aprovação obrigatória, sem push direto

## Convenção de commits

Seguimos [Conventional Commits](https://www.conventionalcommits.org):

- `feat`: nova funcionalidade
- `fix`: correção de bug
- `chore`: tarefas de manutenção e configuração
- `docs`: documentação
- `refactor`: refatoração sem mudança de comportamento
- `test`: adição ou correção de testes

## Como rodar localmente

A definir após scaffold.

## Documentação

Os documentos previstos em `/docs`:

- Diagrama Entidade-Relacionamento (DER)
- Casos de uso
- Regras de negócio
- Contrato de API (OpenAPI/Swagger)
- Templates de mensagens WhatsApp

## Time

| Membro | Papel                   |
|--------|-------------------------|
| Pedro  | Infra, segurança, cloud |
| Tacin  | Dev + IA                |
| Luccas | Dev + IA                |
| Junior | Dados                   |
| Erik   | Dados + IA              |

## Contexto

Projeto acadêmico do CESAR School com potencial de virar startup.
