# Sistema de Triagem Hematológica

## Visão do projeto e motivação

Sistema web para apoiar a regulação de encaminhamentos hematológicos no SUS. A proposta reduz subjetividade por meio de critérios padronizados, pontuação automática de prioridade e revisão auditável por reguladores.

## Explicação geral do sistema

A aplicação permite que unidades solicitantes cadastrem pacientes e encaminhamentos com sinais clínicos e exames laboratoriais. O backend calcula automaticamente a prioridade com base em regras explícitas. Reguladores revisam, confirmam ou ajustam a prioridade com justificativa. Administradores gerenciam usuários e acessam indicadores gerais.

**Perfis de usuário:**
- **Solicitante (`requester`)**: cadastra pacientes e encaminhamentos
- **Regulador (`regulator`)**: avalia e define prioridade final
- **Administrador (`admin`)**: gerencia usuários e visualiza todo o sistema

**Solicitante vs. Regulador:** o solicitante cria e envia encaminhamentos da unidade de saúde (pacientes, exames e critérios clínicos). O regulador analisa o que foi enviado, confirma ou ajusta a prioridade e decide aprovar, devolver, agendar ou cancelar — sempre com justificativa quando necessário.

**Stack:** FastAPI + SQLAlchemy + PostgreSQL (backend) | Next.js + Zustand + Tailwind (frontend)

## Diagrama de arquitetura

```
┌─────────────────┐     JWT/REST      ┌─────────────────┐
│   Next.js       │ ◄──────────────► │   FastAPI       │
│   (Zustand)     │                   │   (SQLAlchemy)  │
└─────────────────┘                   └────────┬────────┘
                                               │
                                               ▼
                                      ┌─────────────────┐
                                      │   PostgreSQL    │
                                      └─────────────────┘
```

## Setup via Docker

### Pré-requisitos

- Docker e Docker Compose instalados

### Passos

1. Clone o repositório e entre na pasta do projeto.

2. Copie as variáveis de ambiente:
   ```bash
   cp .env.example .env
   ```

3. Suba os serviços:
   ```bash
   docker compose up -d
   ```

4. Execute as migrations:
   ```bash
   docker compose exec backend alembic upgrade head
   ```

5. Popule dados de demonstração:
   ```bash
   docker compose exec backend python -m app.seed
   ```

6. Acesse:
   - **Frontend:** http://localhost:3000
   - **API / Swagger:** http://localhost:8000/docs

### Credenciais de demonstração

| Perfil      | E-mail                    | Senha      |
|-------------|---------------------------|------------|
| Admin       | admin@example.com          | admin123   |
| Solicitante | solicitante@example.com    | solicit123 |
| Regulador   | regulador@example.com      | regul123   |

## Checklist de funcionalidades para avaliação

- [ ] Login/logout funcional com JWT
- [ ] CRUD completo de pacientes e encaminhamentos
- [ ] Registro de critérios clínicos e exames laboratoriais
- [ ] Cálculo automático de prioridade por regras explícitas
- [ ] Revisão do regulador com justificativa e histórico
- [ ] Roles `admin`, `requester` e `regulator` respeitadas
- [ ] Dashboard com indicadores por status e prioridade
- [ ] PostgreSQL via Docker Compose
- [ ] Migrations Alembic aplicáveis
- [ ] Swagger documentado em `/docs`
- [ ] Código em inglês, interface em português
