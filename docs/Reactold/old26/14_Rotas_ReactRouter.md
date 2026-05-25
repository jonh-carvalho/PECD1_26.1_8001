---
id: 14_Rotas_ReactRouter
title: 14 - Rotas com React Router
---

# 14 - Rotas com React Router

Vamos adicionar navegação real à nossa aplicação com o React Router: páginas, links, rotas dinâmicas (detalhes do país), página 404, layout compartilhado e navegação programática, mantendo a mesma fonte de dados do Módulo 09 (API do IBGE mapeada para o app).

---

## Objetivos do Módulo

- Configurar o React Router em um app React
- Criar rotas básicas: Home, Favoritos e Detalhes
- Usar Links e NavLink para navegação
- Ler parâmetros de rota com useParams
- Navegar por código com useNavigate
- Implementar layout persistente (Navbar/Footer)
- Tratar rotas inexistentes (404)

---

## 1. Instalação (opcional)

Se ainda não tiver instalado:

```bash
npm install react-router-dom
```

---

## 2. Estrutura de Páginas

- Layout (navbar + conteúdo)
- Home: lista e filtros (do Módulo 11)
- Favoritos: apenas países favoritados (do Módulo 12)
- Detalhes do País: rota dinâmica por código (cca3)
- NotFound: página 404

```
src/
├── App.jsx
├── main.jsx
├── pages/
│   ├── Home.jsx
│   ├── Favorites.jsx
│   ├── CountryDetails.jsx
│   └── NotFound.jsx
├── components/
│   ├── Navbar.jsx
│   └── CountryCard.jsx (com Link p/ detalhes)
└── hooks/
    └── useCountries.js
```

---

## 3. Configuração do Router

```jsx
// src/main.jsx
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
// src/App.jsx
import { Routes, Route } from 'react-router-dom';
import Layout from './components/Layout';
import Home from './pages/Home';
import Favorites from './pages/Favorites';
import CountryDetails from './pages/CountryDetails';
import NotFound from './pages/NotFound';

function App() {
  return (
    <Routes>
      <Route element={<Layout />}> 
        <Route path="/" element={<Home />} />
        <Route path="/favoritos" element={<Favorites />} />
        <Route path="/pais/:code" element={<CountryDetails />} />
        <Route path="*" element={<NotFound />} />
      </Route>
    </Routes>
  );
}

export default App;
```

---

## 4. Layout com Navbar e Outlet

```jsx
// src/components/Layout.jsx
import { NavLink, Outlet } from 'react-router-dom';

function Layout() {
  const linkClass = ({ isActive }) => 
    `nav-link ${isActive ? 'active' : ''}`;

  return (
    <div className="app">
      <nav className="navbar">
        <div className="brand">🌍 Países</div>
        <div className="links">
          <NavLink to="/" className={linkClass}>Início</NavLink>
          <NavLink to="/favoritos" className={linkClass}>Favoritos</NavLink>
        </div>
      </nav>

      <main className="container">
        <Outlet />
      </main>

      <footer className="footer">
        <p>
          Dados por <a href="https://servicodados.ibge.gov.br/api/docs" target="_blank" rel="noreferrer">API do IBGE</a>
        </p>
      </footer>
    </div>
  );
}

export default Layout;
```

---

## 5. Home: lista com links para detalhes

Reaproveite os dados do hook `useCountries` e a filtragem do Módulo 11.

```jsx
// src/pages/Home.jsx
import { useEffect, useState } from 'react';
import { Link } from 'react-router-dom';
import useCountries from '../hooks/useCountries';

function Home() {
  const { countries, isLoading, error } = useCountries();
  const [search, setSearch] = useState('');
  const filtered = countries.filter(c => 
    c.name.toLowerCase().includes(search.toLowerCase())
  );

  if (isLoading) return <p>Carregando...</p>;
  if (error) return <p>Erro: {error}</p>;

  return (
    <div>
      <h1>Lista de Países</h1>
      <input 
        placeholder="Buscar país..." 
        value={search} 
        onChange={(e) => setSearch(e.target.value)}
      />

      <div className="countries-grid">
        {filtered.map(country => (
          <div key={country.cca3} className="country-card">
            <img src={country.flag} alt={country.name} />
            <h3>{country.name}</h3>
            <p>🌎 {country.region}</p>
            <Link 
              to={`/pais/${country.cca3}`} 
              state={{ country }}
            >
              Ver detalhes
            </Link>
          </div>
        ))}
      </div>
    </div>
  );
}

export default Home;
```

Dica: passamos o país via state no Link para evitar nova busca e manter consistência com os dados já carregados da API do IBGE.

---

## 6. Favoritos: rota dedicada

Reaproveite a lógica do Módulo 12 (localStorage + toggle).

```jsx
// src/pages/Favorites.jsx
import { useEffect, useState } from 'react';
import { Link } from 'react-router-dom';
import useCountries from '../hooks/useCountries';

function Favorites() {
  const { countries, isLoading, error } = useCountries();
  const [favorites, setFavorites] = useState([]);

  useEffect(() => {
    const saved = localStorage.getItem('countryFavorites');
    if (saved) setFavorites(JSON.parse(saved));
  }, []);

  const favoriteCountries = countries.filter(c => 
    favorites.includes(c.cca3)
  );

  if (isLoading) return <p>Carregando...</p>;
  if (error) return <p>Erro: {error}</p>;

  return (
    <div>
      <h1>Favoritos ({favoriteCountries.length})</h1>
      {favoriteCountries.length === 0 ? (
        <p>Nenhum favorito. Volte à página inicial e marque alguns! 🙂</p>
      ) : (
        <ul className="favorites-list">
          {favoriteCountries.map(c => (
            <li key={c.cca3}>
              <Link to={`/pais/${c.cca3}`} state={{ country: c }}>
                <img src={c.flag} alt={c.name} width="20" /> {c.name}
              </Link>
            </li>
          ))}
        </ul>
      )}
    </div>
  );
}

export default Favorites;
```

---

## 7. Detalhes do País: rota dinâmica

Usaremos useParams para ler o :code. Se o país vier no state, usamos direto; caso contrário, buscamos na lista já carregada pelo hook useCountries (que já usa a API do IBGE).

```jsx
// src/pages/CountryDetails.jsx
import { useMemo } from 'react';
import { useLocation, useNavigate, useParams } from 'react-router-dom';
import useCountries from '../hooks/useCountries';

function CountryDetails() {
  const { code } = useParams();
  const navigate = useNavigate();
  const location = useLocation();
  const countryFromState = location.state?.country;
  const { countries, isLoading, error } = useCountries();

  const country = useMemo(() => {
    if (countryFromState) return countryFromState;
    return countries.find((item) => item.cca3 === code?.toUpperCase()) || null;
  }, [countryFromState, countries, code]);

  const population = useMemo(() => {
    if (!country || !country.population) return 'N/A';
    return new Intl.NumberFormat('pt-BR').format(country.population);
  }, [country]);

  if (!countryFromState && isLoading) return <p>Carregando detalhes...</p>;
  if (!country && error) return <p>Erro: {error}</p>;
  if (!country) return <p>País não encontrado.</p>;

  return (
    <div>
      <button onClick={() => navigate(-1)}>⬅️ Voltar</button>
      <h1>{country.name}</h1>
      <img src={country.flag} alt={country.name} />
      <ul>
        <li>📍 Capital: {country.capital || 'N/A'}</li>
        <li>🌎 Região: {country.region} ({country.subregion})</li>
        <li>👥 População: {population}</li>
        <li>📏 Área: {country.area ? `${new Intl.NumberFormat('pt-BR').format(country.area)} km²` : 'N/A'}</li>
      </ul>
    </div>
  );
}

export default CountryDetails;
```

---

## 8. Página 404 (NotFound)

```jsx
// src/pages/NotFound.jsx
import { Link } from 'react-router-dom';

export default function NotFound() {
  return (
    <div style={{ textAlign: 'center', padding: 40 }}>
      <h1>404 - Página não encontrada</h1>
      <p>A rota acessada não existe.</p>
      <Link to="/">Voltar para a Home</Link>
    </div>
  );
}
```
