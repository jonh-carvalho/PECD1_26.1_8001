---
id: 14_Rotas_ReactRouter
title: 14 - Rotas com React Router
---

# 14 - Rotas com React Router

Neste módulo, vamos transformar a aplicação em uma SPA com páginas reais. O foco é navegação entre tela inicial, favoritos e detalhes do país.

## Objetivos do Módulo

- Configurar BrowserRouter
- Criar rotas para Home, Favoritos e Detalhes
- Usar Link e NavLink para navegação
- Ler parâmetros com useParams
- Tratar rota inexistente com página 404

## Pré-requisitos

- Lista de países funcional
- Favoritos e filtros consolidados
- Estrutura de componentes básica

## Conceito Principal

Rotas permitem separar responsabilidades por página e melhorar a organização da aplicação sem recarregar o navegador.

## Exemplo Guiado

```jsx
// main.jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

```jsx
// App.jsx
import { Route, Routes } from 'react-router-dom';
import Home from './pages/Home';
import Favorites from './pages/Favorites';
import CountryDetails from './pages/CountryDetails';
import NotFound from './pages/NotFound';

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/favoritos" element={<Favorites />} />
      <Route path="/pais/:code" element={<CountryDetails />} />
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
}

export default App;
```

## Exercício Prático

1. Adicione um menu com links para Home e Favoritos.
2. Crie uma rota de detalhes com parâmetro :code.
3. Implemente página 404 com botão de voltar ao início.

## Erros Comuns

- Usar Link fora de BrowserRouter
- Definir caminho dinâmico sem :code
- Não tratar rota * para páginas inexistentes

## Próximo Passo

No próximo módulo você vai finalizar a interface com estilização avançada e consistência visual.
