# Instalação

## Pré-requisitos

| Ferramenta | Versão mínima |
|------------|---------------|
| Node.js    | 20.x          |
| npm        | 10.x          |
| Git        | 2.x           |

---

## 1. Clonar o repositório

```bash
gh repo clone prbretas/bookshelf-api
cd bookshelf-api
```

Ou via HTTPS:

```bash
git clone https://github.com/prbretas/bookshelf-api.git
cd bookshelf-api
```

---

## 2. Backend

```bash
cd backend
npm ci          # instala dependências exatas do package-lock.json
npm start       # inicia em http://localhost:3000
```

### Variáveis de ambiente

| Variável | Padrão | Descrição              |
|----------|--------|------------------------|
| `PORT`   | `3000` | Porta do servidor HTTP |

Crie um arquivo `.env` na raiz de `backend/` se quiser sobrescrever:

```env
PORT=4000
```

### Verificar se está rodando

```bash
curl http://localhost:3000/health
# {"status":"ok","service":"bookshelf-api"}
```

---

## 3. Frontend

```bash
cd frontend
npm ci
npm run dev     # inicia em http://localhost:5173
```

---

## 4. Rodar testes

```bash
# Backend
cd backend
npm test

# Frontend
cd frontend
npm test
```

---

## 5. Gerar build de produção

```bash
# Backend — valida sintaxe
cd backend && npm run build

# Frontend — gera bundle em frontend/dist/
cd frontend && npm run build
```

---

## 6. Lint

```bash
cd backend  && npm run lint
cd frontend && npm run lint
```

---

## Estrutura de portas padrão

| Serviço  | URL                     |
|----------|-------------------------|
| Backend  | http://localhost:3000   |
| Frontend | http://localhost:5173   |
