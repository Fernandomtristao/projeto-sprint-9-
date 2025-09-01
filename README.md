# Projeto 9 — Projeto Final (Sprint 9)

## 📌 Contexto
Descreva em 3–5 linhas **o problema**, **para quem** e **o que este projeto entrega**.  
Ex.: *Este projeto implementa uma aplicação/serviço para ___ que permite ___, focado em ___, visando reduzir/automatizar/medir ___.*

## 🎯 Objetivos do Projeto
- **Objetivo 1:** <!-- ex.: disponibilizar API para cadastro -->
- **Objetivo 2:** <!-- ex.: dashboard com métricas em tempo real -->
- **Objetivo 3:** <!-- ex.: pipeline CI/CD e suíte de testes -->

### Critérios de Aceite (DoR/DoD)
- [ ] Requisitos funcionais completos e testados
- [ ] Cobertura de testes ≥ **80%** (unidade) e **70%** (integração)
- [ ] Pipeline CI executando *lint*, *tests* e *build* sem falhas
- [ ] Documentação atualizada (este README + API + decisões)

---

## 🏗️ Arquitetura & Tecnologias
**Stack sugerida (ajuste conforme seu projeto):**
- **Backend:** Node.js / Express *ou* Python / FastAPI
- **Frontend:** React / Vite / Tailwind
- **Banco de Dados:** PostgreSQL *ou* MongoDB
- **Mensageria/Cache:** Redis / RabbitMQ (opcional)
- **Infra:** Docker & Docker Compose
- **Observabilidade:** Prometheus, Grafana, OpenTelemetry (opcional)

**Diagrama (alto nível):**
```
[Frontend] ⇄ [API Gateway/Backend] ⇄ [DB]
               ↳ [Mensageria/Cache]
               ↳ [Serviços de Observabilidade]
```

> **Decisões de Arquitetura (ADR):**
> - ADR-001: linguagem/stack escolhida — _por quê_
> - ADR-002: persistência e modelo de dados — _por quê_
> - ADR-003: estratégia de logs/telemetria — _por quê_

---

## 🧩 Estrutura de Pastas
```
projeto-final/
├─ docs/                 # Documentação (ADR, diagramas, coleções HTTP)
├─ src/                  # Código-fonte
│  ├─ app/               # Camada de aplicação (casos de uso)
│  ├─ domain/            # Entidades, regras de negócio
│  ├─ infra/             # Adapters (DB, HTTP, Mensageria)
│  └─ tests/             # Testes (unit, integration, e2e)
├─ scripts/              # Scripts utilitários (migrações, seeds, etc.)
├─ .editorconfig         # Padrões de editor
├─ .gitignore
├─ Dockerfile
├─ docker-compose.yml
├─ package.json / pyproject.toml
└─ README.md
```

---

## ⚙️ Pré-requisitos
- **Git** ≥ 2.40
- **Docker** ≥ 24 e **Docker Compose** ≥ 2.25
- **Node.js** ≥ 20 *ou* **Python** ≥ 3.11
- **Make** (opcional, para facilitar comandos)

> Verifique as versões com `git --version`, `docker -v`, `node -v` ou `python --version`.

---

## 🚀 Como rodar localmente
### Usando Docker (recomendado)
```bash
# 1) Copie variáveis de ambiente
aut cp .env.example .env

# 2) Suba os serviços
docker compose up --build

# 3) Acesse
# Backend: http://localhost:3000 ou 8000
# Frontend: http://localhost:5173
# Docs API (Swagger/Redoc): http://localhost:3000/docs
```

### Sem Docker (Node.js — exemplo)
```bash
# Instalação
yarn install  # ou npm ci

# Variáveis de ambiente
cp .env.example .env

# Banco (opcional)
yarn prisma migrate dev  # ou scripts/migrate.sh

# Lint & Testes
yarn lint
yarn test

# Executar
yarn dev
```

### Sem Docker (Python/FastAPI — exemplo)
```bash
# Crie venv e instale dependências
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# Variáveis de ambiente
cp .env.example .env

# Lint & Testes
ruff check .
pytest -q

# Executar
uvicorn src.main:app --reload --port 8000
```

---

## 🔧 Configuração
Crie um arquivo `.env` a partir do `.env.example` e preencha as chaves:
```
APP_PORT=3000
DATABASE_URL=postgres://user:pass@localhost:5432/db
JWT_SECRET=troque-isto
LOG_LEVEL=info
```
> Nunca versione segredos. Use `.gitignore`.

---

## 🧪 Qualidade & Testes
- **Tipos de teste:** unidade, integração, e2e/contrato
- **Cobertura mínima:** 80% unidade / 70% integração
- **Relatórios:** `coverage/` publicado no pipeline
- **Análise estática:** ESLint/Prettier (JS) ou Ruff/Mypy (Python)
- **Segurança:** `npm audit`/`pip-audit` e varredura SAST (ex.: Semgrep)

**Comandos exemplo (Node.js):**
```bash
yarn test --coverage
```
**Comandos exemplo (Python):**
```bash
pytest --cov=src --cov-report=term-missing
```

---

## 🧭 Padrões de Código
- **Variáveis:** `snake_case` descritivo
- **Constantes:** `UPPER_SNAKE_CASE`
- **Comentários:** apenas para blocos relevantes (evitar comentários óbvios)
- **Organização:** modular; blocos reutilizáveis importados conforme necessidade
- **Commits:** Conventional Commits (`feat:`, `fix:`, `chore:` …)
- **Branching:** Git Flow simplificado (`main`, `develop`, `feature/*`)

**Exemplo de Conventional Commit:**
```
feat(api): adiciona endpoint de criação de usuário
```

---

## 📈 Observabilidade
- **Logs estruturados** (JSON), níveis: `debug` | `info` | `warn` | `error`
- **Métricas** via Prometheus (ex.: latência, taxa de erro)
- **Tracing distribuído** com OpenTelemetry (opcional)

---

## 🔄 CI/CD (exemplo com GitHub Actions)
```yaml
name: ci
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: yarn install --frozen-lockfile
      - run: yarn lint && yarn test --coverage
      - run: docker build -t app:ci .
```

---

## 🗃️ Banco de Dados (exemplo)
- **Modelo:** <!-- ex.: relacional (PostgreSQL) -->
- **Migrações:** `scripts/migrate.sh` ou Prisma/Alembic
- **Seeds:** `scripts/seed.sh`

**Diagrama ER (resumo):**
```
User (id, name, email)
Project (id, name, owner_id)
Task (id, project_id, title, status)
```

---

## 🧰 Scripts Úteis
```bash
make up          # subir containers\ nmake down        # derrubar containers
make logs        # logs do serviço
make test        # roda testes
make lint        # análise estática
```

---

## 🗺️ Roadmap (Sprint 9)
- [ ] Definir backlog priorizado
- [ ] Implementar MVP funcional
- [ ] Cobrir fluxos críticos com testes e2e
- [ ] Publicar imagem Docker e documentação
- [ ] Preparar apresentação e demo final

---

## ✅ Checklist de Entrega
- [ ] Funcionalidades principais entregues
- [ ] Testes automatizados com cobertura
- [ ] Pipelines passando
- [ ] README completo e atualizado
- [ ] Demonstração gravada (opcional)

---

## 🤝 Contribuição
1. Crie uma *issue* descrevendo a mudança
2. Crie uma *branch* `feature/sua-mudanca`
3. Abra um *Pull Request* com descrição clara e evidências (prints, logs, links de build)

---

## 📄 Licença
Este projeto está licenciado sob a **MIT License**. Veja o arquivo `LICENSE` para mais detalhes.

---

## 📚 Referências
- ADRs e diagramas em `docs/`
- Coleções de API (Insomnia/Postman) em `docs/collections/`
- Confluence/Notion (se aplicável)

> **Dica:** Mantenha este README como a fonte de verdade para iniciar o projeto rapidamente. Atualize sempre que decisões ou fluxos mudarem.

