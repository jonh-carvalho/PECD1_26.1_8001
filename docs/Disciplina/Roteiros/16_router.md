Este roteiro foi desenvolvido para guiá-lo, passo a passo, na construção de uma base sólida em navegação com React Router. Seguindo uma progressão lógica, você sairá do zero absoluto até a criação de uma aplicação funcional com páginas, navegação e layouts estruturados.

---

### 📚 Roteiro de Aprendizagem: React Router do Básico ao Funcional

Este roteiro é prático e dividido em missões. Cada missão introduz um conceito fundamental, culminando em um projeto funcional.

#### Missão 0: Preparação do Ambiente

Antes de começar, você precisa preparar o terreno.

- **Objetivo**: Criar um novo projeto React e instalar a biblioteca de rotas.
- **Passo a passo**:
    1.  No terminal, crie um novo projeto React (usando Vite ou CRA):
        ```bash
        npm create vite@latest meu-app-rotas -- --template react
        cd meu-app-rotas
        ```
    2.  Instale o `react-router-dom`, a biblioteca que usaremos:
        ```bash
        npm install react-router-dom
        ```
    3.  Para manter a organização, crie as pastas `pages` e `components` dentro da pasta `src` .
        ```text
        meu-app-rotas/
        └── src/
            ├── components/
            └── pages/
        ```

#### Missão 1: Habilitando o Roteamento

A primeira missão é "ligar" o roteador na sua aplicação.

- **Objetivo**: Envolver a aplicação com o `BrowserRouter` para que ela entenda as rotas.
- **Conceitos-chave**: O `BrowserRouter` é o "cérebro" da operação. Ele usa a URL da barra de endereços para decidir o que mostrar na tela, sem recarregar a página .
- **Prática**: Abra o arquivo `src/main.jsx` (ou `index.js`) e importe o `BrowserRouter` do `react-router-dom`. Envolva o componente `<App />` com ele .

```jsx
// src/main.jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom'; // Importe o BrowserRouter
import App from './App';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <BrowserRouter> {/* Envolva o App */}
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

#### Missão 2: Criando suas Primeiras Páginas

Agora que o roteador está ativo, você precisa criar os conteúdos que serão navegados.

- **Objetivo**: Criar componentes simples que representarão as páginas do seu site.
- **Conceitos-chave**: Uma "página" em uma SPA (Single Page Application) é apenas um componente React como qualquer outro, mas que será renderizado em uma rota específica .
- **Prática**: Dentro da pasta `src/pages`, crie dois arquivos: `Home.jsx` e `Sobre.jsx`.

```jsx
// src/pages/Home.jsx
function Home() {
  return <h1>🏠 Página Inicial</h1>;
}
export default Home;
```

```jsx
// src/pages/Sobre.jsx
function Sobre() {
  return <h1>📖 Sobre Nós</h1>;
}
export default Sobre;
```

#### Missão 3: Configurando as Rotas

Com as páginas criadas, é hora de mapear cada uma para um caminho (path) da URL.

- **Objetivo**: Configurar as rotas da aplicação usando os componentes `Routes` e `Route`.
- **Conceitos-chave**:
    - `<Routes>`: É um componente "filtro" que olha para a URL atual e renderiza o primeiro `<Route>` que corresponder .
    - `<Route>`: Cada rota é definida por ele. A prop `path` define o caminho na URL (ex: `/sobre`), e a prop `element` define qual componente será renderizado naquele caminho .
- **Prática**: No arquivo `src/App.jsx`, importe os componentes `Routes`, `Route` e suas páginas. Dentro do JSX do `App`, use `<Routes>` para agrupar suas `<Route>`s.

```jsx
// src/App.jsx
import { Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import Sobre from './pages/Sobre';

function App() {
  return (
    <div>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/sobre" element={<Sobre />} />
      </Routes>
    </div>
  );
}
export default App;
```

**Teste**: Rode o projeto com `npm run dev` e acesse `http://localhost:5173/` e depois `http://localhost:5173/sobre`. Você verá o conteúdo de cada página sendo trocado de acordo com a URL.

#### Missão 4: Adicionando Navegação

Ter as páginas é ótimo, mas o usuário precisa conseguir navegar entre elas sem digitar a URL manualmente.

- **Objetivo**: Criar links de navegação usando o componente `Link`.
- **Conceitos-chave**: Diferente da tag `<a>` do HTML, o `<Link>` do React Router evita o recarregamento da página, mantendo a performance e o estado da sua aplicação. A navegação é feita através da prop `to`, que deve conter o mesmo `path` definido no `Route` .
- **Prática**: Crie um novo componente chamado `Menu.jsx` dentro da pasta `src/components` e adicione os links.

```jsx
// src/components/Menu.jsx
import { Link } from 'react-router-dom';

function Menu() {
  return (
    <nav>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/sobre">Sobre</Link></li>
      </ul>
    </nav>
  );
}
export default Menu;
```

Agora, importe o `Menu` no `App.jsx` e coloque-o acima do `<Routes>`. Assim, ele ficará visível em todas as "páginas" .

```jsx
// src/App.jsx (atualizado)
import { Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import Sobre from './pages/Sobre';
import Menu from './components/Menu'; // Importe o Menu

function App() {
  return (
    <div>
      <Menu /> {/* Menu visível globalmente */}
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/sobre" element={<Sobre />} />
      </Routes>
    </div>
  );
}
export default App;
```

#### Missão 5: Criando um Layout Padrão (Bônus)

Por fim, para um projeto mais profissional, você aprenderá a criar um layout que se repete em várias páginas, evitando repetição de código.

- **Objetivo**: Criar um componente de `Layout` que serve como um template para as páginas.
- **Conceitos-chave**: O componente `<Outlet />` age como um "placeholder". Dentro do seu componente de `Layout`, você define tudo o que é comum (menu, rodapé) e onde o `<Outlet />` estiver, o conteúdo da página ativa (Home, Sobre, etc.) será renderizado .
- **Prática**:
    1.  Crie um novo arquivo `Layout.jsx` dentro da pasta `src/components`.
    2.  Mova o componente `Menu` e o `<Outlet />` para dentro desse `Layout`.
    3.  Volte ao `App.jsx` e transforme a rota raiz (`/`) em uma rota pai, que tem o `Layout` como `element` e as outras rotas (`/`, `/sobre`) como filhos.

```jsx
// src/components/Layout.jsx
import { Outlet } from 'react-router-dom';
import Menu from './Menu';

function Layout() {
  return (
    <div>
      <Menu />
      <main>
        <Outlet /> {/* O conteúdo da página atual aparecerá aqui */}
      </main>
      <footer>Rodapé do site</footer>
    </div>
  );
}
export default Layout;
```

```jsx
// src/App.jsx (versão final com Layout)
import { Routes, Route } from 'react-router-dom';
import Layout from './components/Layout';
import Home from './pages/Home';
import Sobre from './pages/Sobre';

function App() {
  return (
    <Routes>
      {/* Rota pai, que define o layout padrão */}
      <Route path="/" element={<Layout />}>
        {/* Rotas filhas, que serão renderizadas dentro do <Outlet /> do Layout */}
        <Route index element={<Home />} /> {/* index = path="/" */}
        <Route path="sobre" element={<Sobre />} />
      </Route>
    </Routes>
  );
}
export default App;
```

Com este roteiro, você terá um conhecimento prático e sólido dos fundamentos do React Router, pronto para construir aplicações web mais completas e com navegação fluida.