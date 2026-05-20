# 📚 BookShelf API

Aplicação full-stack para gerenciamento de uma estante digital de livros.  
Permite cadastrar, listar, filtrar, atualizar o status e remover livros, além de exibir métricas da coleção.

---

## 🗂️ Estrutura do projeto

```
bookshelf/
├── backend/          # API REST — Node.js + Express
│   ├── src/
│   │   ├── app.js    # Rotas e lógica de negócio
│   │   └── server.js # Inicialização do servidor
│   ├── tests/
│   │   └── books.test.js
│   ├── openapi.yaml  # Contrato OpenAPI 3.0
│   └── package.json
├── frontend/         # Interface — React + Vite
│   ├── src/
│   │   ├── App.jsx
│   │   ├── components/BookCard.jsx
│   │   └── pages/
│   │       ├── Dashboard.jsx
│   │       ├── BookList.jsx
│   │       └── NewBook.jsx
│   └── package.json
├── docs/             # Documentação técnica
│   ├── ARCHITECTURE.md
│   ├── INSTALLATION.md
│   └── CLASSROOM_CHALLENGES.md
└── .github/
    └── workflows/
        └── ci.yml    # Pipeline CI/CD
```

---

## 🚀 Início rápido

```bash
# Backend
cd backend
npm ci
npm start          # http://localhost:3000

# Frontend (outro terminal)
cd frontend
npm ci
npm run dev        # http://localhost:5173
```

---

## 🔌 Endpoints da API

| Método   | Rota                  | Descrição                        |
|----------|-----------------------|----------------------------------|
| `GET`    | `/health`             | Status da API                    |
| `GET`    | `/books`              | Lista livros (filtros opcionais) |
| `POST`   | `/books`              | Cadastra novo livro              |
| `GET`    | `/books/:id`          | Busca livro por ID               |
| `PATCH`  | `/books/:id/status`   | Atualiza status do livro         |
| `DELETE` | `/books/:id`          | Remove livro                     |
| `GET`    | `/metrics`            | Métricas da estante              |

### Filtros disponíveis em `GET /books`

| Parâmetro  | Valores aceitos                          |
|------------|------------------------------------------|
| `status`   | `unread` · `reading` · `finished`        |
| `category` | `software` · `architecture` · `data` · `career` |

---

## 📦 Modelo de livro

```json
{
  "id": 1,
  "title": "Clean Code",
  "author": "Robert C. Martin",
  "category": "software",
  "status": "reading",
  "rating": 5,
  "createdAt": "2026-05-01T10:00:00.000Z"
}
```

---

## 🧪 Testes e qualidade

```bash
# Backend
cd backend
npm run lint   # ESLint
npm test       # Jest + Supertest

# Frontend
cd frontend
npm run lint   # ESLint
npm test       # Vitest
```

---

## 🏗️ Build

```bash
cd backend  && npm run build   # Valida sintaxe dos arquivos JS
cd frontend && npm run build   # Gera bundle em frontend/dist/
```

---

## ⚙️ CI/CD

O pipeline `.github/workflows/ci.yml` executa automaticamente em `push` e `pull_request` para `main`:

1. **backend** — lint → test → build
2. **frontend** — lint → test → build
3. **docs** — valida `openapi.yaml` + verifica arquivos obrigatórios
4. **deploy** — só roda se os três jobs anteriores passarem (apenas em push para `main`)

---

## 📄 Documentação adicional

- [Instalação detalhada](docs/INSTALLATION.md)
- [Arquitetura](docs/ARCHITECTURE.md)
- [Contrato OpenAPI](backend/openapi.yaml)
