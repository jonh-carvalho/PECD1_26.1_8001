# Gestor de Eventos - Front-end React

Aplicação visual inspirada no estilo de dashboard do site de referência, com cadastro e listagem de eventos.

## Stack

- React + Vite
- JSON Server para API local
- CSS customizado

## Como executar

1. Instale as dependências:

```bash
npm install
```

2. Rode front-end e API juntos:

```bash
npm run dev:all
```

3. Abra no navegador:

- Front-end: http://localhost:5173
- API local: http://localhost:3001/events

## Scripts

- `npm run dev`: inicia apenas o Vite
- `npm run server`: inicia apenas o JSON Server
- `npm run dev:all`: inicia Vite + JSON Server
- `npm run lint`: executa ESLint
- `npm run build`: gera build de produção

## Estrutura principal

- `src/App.jsx`: orquestra as visões e integra os serviços
- `src/components`: componentes de layout, cards, lista e formulário
- `src/services.js`: chamadas HTTP para API local
- `db.json`: base inicial de eventos

## Fluxo disponível nesta versão

- Visual de dashboard com sidebar
- KPIs de eventos, inscritos e horas
- Listagem de eventos
- Cadastro de novo evento com persistência na API local
