# Arquitetura

## Visão geral

O BookShelf é uma aplicação full-stack composta por um backend REST em Node.js e um frontend em React. Os dois serviços são independentes e se comunicam via HTTP.

```
┌─────────────────────────────────────────────────────┐
│                     Usuário                         │
└──────────────────────┬──────────────────────────────┘
                       │ HTTP (browser)
┌──────────────────────▼──────────────────────────────┐
│              Frontend — React + Vite                │
│  Dashboard · BookList · NewBook · BookCard          │
│  porta 5173 (dev) / dist/ (produção)                │
└──────────────────────┬──────────────────────────────┘
                       │ HTTP REST (JSON)
┌──────────────────────▼──────────────────────────────┐
│              Backend — Node.js + Express            │
│  /health · /books · /books/:id · /metrics           │
│  porta 3000                                         │
└──────────────────────┬──────────────────────────────┘
                       │ in-memory (array)
┌──────────────────────▼──────────────────────────────┐
│              Dados — memória (runtime)              │
│  books[] inicializado com 3 livros de exemplo       │
└─────────────────────────────────────────────────────┘
```

---

## Backend

### Tecnologias

| Pacote      | Função                        |
|-------------|-------------------------------|
| Express     | Framework HTTP                |
| cors        | Habilita CORS para o frontend |
| Jest        | Runner de testes              |
| Supertest   | Testes de integração HTTP     |
| ESLint      | Análise estática de código    |

### Camadas

```
backend/src/
├── app.js      # Express app: rotas, validações, lógica de negócio
└── server.js   # Ponto de entrada: bind de porta e listen
```

### Rotas e regras de negócio

| Rota                  | Método   | Validações                                              |
|-----------------------|----------|---------------------------------------------------------|
| `/health`             | GET      | —                                                       |
| `/books`              | GET      | `status` e `category` devem ser valores permitidos      |
| `/books`              | POST     | `title` e `author` obrigatórios; `category` e `status` devem ser valores permitidos |
| `/books/:id`          | GET      | Retorna 404 se não encontrado                           |
| `/books/:id/status`   | PATCH    | `status` deve ser valor permitido; 404 se não encontrado |
| `/books/:id`          | DELETE   | 404 se não encontrado; retorna 204 sem corpo            |
| `/metrics`            | GET      | Calcula totais, agrupamentos e média de rating          |

### Valores permitidos

```
status:   unread | reading | finished
category: software | architecture | data | career
rating:   0 – 5
```

### Persistência

Os dados vivem em um array em memória (`let books = [...]`). Ao reiniciar o servidor os dados voltam ao estado inicial. Não há banco de dados nesta versão.

---

## Frontend

### Tecnologias

| Pacote                  | Função                        |
|-------------------------|-------------------------------|
| React 18                | UI declarativa                |
| Vite 5                  | Bundler e dev server          |
| Vitest                  | Runner de testes              |
| Testing Library / React | Testes de componentes         |
| ESLint                  | Análise estática de código    |

### Componentes

```
frontend/src/
├── App.jsx              # Raiz: dados mockados + layout geral
├── components/
│   └── BookCard.jsx     # Card individual de livro
└── pages/
    ├── Dashboard.jsx    # Métricas (total, por status)
    ├── BookList.jsx     # Lista de BookCards
    └── NewBook.jsx      # Formulário de cadastro (estático)
```

> O frontend desta versão usa dados mockados diretamente no `App.jsx`. A integração com a API via `fetch` é o próximo passo natural de evolução.

---

## Pipeline CI/CD

```
push / pull_request → main
        │
        ├── job: backend  (lint → test → build)
        ├── job: frontend (lint → test → build)
        ├── job: docs     (valida openapi.yaml + arquivos obrigatórios)
        │
        └── job: deploy   (needs: backend + frontend + docs)
                          (apenas em push para main)
```

Arquivo: `.github/workflows/ci.yml`

---

## Diagrama de fluxo da API

Veja também: [`docs/diagrams/api-flow.md`](diagrams/api-flow.md)
