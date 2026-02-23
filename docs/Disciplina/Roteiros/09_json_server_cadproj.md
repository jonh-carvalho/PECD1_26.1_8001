---
title: Roteiro — JSON Server no CadProjJsonServer
---

# Objetivo

Guia rápido para instalar, configurar e usar o JSON Server como API fake do app localizado em `src/CadProjJsonServer`, expondo os dados do arquivo `db.json`.

> Ambiente: Windows, PowerShell (`pwsh`). Frontend Vite/React em `src/CadProjJsonServer/gestor-projetos`.

## 1. Pré‑requisitos
- Node.js e npm instalados (versão LTS recomendada).
- A pasta do projeto: `src/CadProjJsonServer` contém o `db.json` com a coleção `projects`.

Estrutura relevante:

```
src/CadProjJsonServer/
  db.json            # base de dados do JSON Server
  gestor-projetos/   # app React (Vite)
```

## 2. Instalação do JSON Server

Opção A — usar npx (não instala nada permanente):

```powershell
cd src/CadProjJsonServer
npx json-server --version
```

Opção B — instalar localmente (recomendado para scripts de projeto):

```powershell
cd src/CadProjJsonServer
npm install -D json-server
```

## 3. Subir a API fake

Ainda dentro de `src/CadProjJsonServer`, rode um dos comandos abaixo.

Usando npx (sem instalação):

```powershell
npx json-server --watch db.json --port 8000 --delay 300
```

Usando instalação local (se fez `npm i -D json-server`):

```powershell
.\u005cnode_modulesjson-serverbinjson-server.js --watch db.json --port 8000 --delay 300
```

Observações:
- Porta sugerida: `8000` (o Vite geralmente roda em `5173`).
- `--delay 300` adiciona latência simulada (opcional) para testes.

Ao iniciar, a API ficará acessível em:

- Lista de projetos: `http://localhost:8000/projects`
- Projeto por id: `http://localhost:8000/projects/1`

## 4. O arquivo db.json

Exemplo real do repositório (`src/CadProjJsonServer/db.json`):

```json
{
  "projects": [
    { "id": "1",   "name": "Projeto Alpha", "description": "...", "budget": 50000 },
    { "id": "2",   "name": "Aplicativo Móvel de Fitness", "description": "...", "budget": 85000 },
    { "id": "2bda","name": "tese", "description": "tese", "budget": 2332 },
    { "id": "e001","name": "tstete", "description": "teste", "budget": 3232 }
  ]
}
```

## 5. Endpoints disponíveis (REST)

- `GET /projects` — lista todos
- `GET /projects/{id}` — obtém um
- `POST /projects` — cria
- `PUT /projects/{id}` — substitui
- `PATCH /projects/{id}` — atualiza parcial
- `DELETE /projects/{id}` — remove

Exemplos com PowerShell (Invoke-RestMethod):

```powershell
# GET lista
irm http://localhost:8000/projects

# GET por id
irm http://localhost:8000/projects/1

# POST (criar)
$body = @{ id = "p100"; name = "Novo"; description = "Exemplo"; budget = 1234 } | ConvertTo-Json
irm http://localhost:8000/projects -Method POST -ContentType 'application/json' -Body $body

# PATCH (atualizar parcial)
$update = @{ budget = 2000 } | ConvertTo-Json
irm http://localhost:8000/projects/p100 -Method PATCH -ContentType 'application/json' -Body $update

# DELETE
irm http://localhost:8000/projects/p100 -Method DELETE
```

## 6. Integração com o app React (Vite)

O app está em `src/CadProjJsonServer/gestor-projetos`. Você tem duas estratégias:

### Opção A — Chamar a URL direta

```jsx
// Exemplo simples com fetch
useEffect(() => {
  fetch('http://localhost:8000/projects')
    .then((r) => r.json())
    .then(setProjects)
    .catch(console.error)
}, [])
```

Prós: simples. Contras: precisa trocar URLs entre ambientes.

### Opção B — Proxy do Vite (recomendado)

Edite `src/CadProjJsonServer/gestor-projetos/vite.config.js` para adicionar proxy:

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  server: {
    proxy: {
      '/api': {
        target: 'http://localhost:8000',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, ''),
      },
    },
  },
})
```

Então, no frontend, chame `'/api/projects'`:

```jsx
useEffect(() => {
  fetch('/api/projects')
    .then((r) => r.json())
    .then(setProjects)
}, [])
```

## 7. Rodando tudo junto

Em dois terminais:

```powershell
# Terminal 1: API fake
cd src/CadProjJsonServer
npx json-server --watch db.json --port 8000

# Terminal 2: frontend
cd src/CadProjJsonServer/gestor-projetos
npm install
npm run dev
```

Abra o app no endereço indicado pelo Vite (ex.: `http://localhost:5173`).

## 8. Scripts úteis (opcional)

Para facilitar, você pode adicionar ao `src/CadProjJsonServer/package.json`:

```json
{
  "scripts": {
    "api": "json-server --watch db.json --port 8000 --delay 300"
  },
  "devDependencies": {
    "json-server": "^1.0.0"
  }
}
```

Depois rode:

```powershell
cd src/CadProjJsonServer
npm run api
```

## 9. Dicas e resolução de problemas

- Porta ocupada: troque `--port 8000` para outra (ex.: `8001`).
- CORS: o JSON Server já habilita CORS; com Proxy Vite, use `fetch('/api/...')`.
- Persistência: alterações gravam diretamente no `db.json`. Versione com cuidado.
- IDs: mantenha `id` único por item para evitar conflitos em rotas.

---

Pronto! Sua API fake está ativa para o `CadProjJsonServer` e pronta para ser consumida pelo app React.
