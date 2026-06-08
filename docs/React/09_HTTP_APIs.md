---
id: 09 - Conectando com APIs Reais
title: 09 - Conectando com APIs Reais
---
# 09 - **Conectando com APIs Reais**

Agora é hora de transformar nossa Lista de Países! Neste módulo, vamos **conectar com a API do IBGE** e carregar dados de **250+ países do mundo**. Sem simulações, sem dados fake - apenas requisições HTTP reais aprendendo na prática!

---

## **Objetivos do Módulo**
- Fazer a primeira requisição HTTP com `fetch()`
- Conectar com a API do IBGE (Instituto Brasileiro de Geografia e Estatística)
- Carregar dados de 250+ países do mundo
- Implementar estados de loading e erro
- Adaptar componentes para dados reais da API
- Mapear estrutura de dados da API para o formato do app

---

## **1. A API do IBGE - Serviço de Dados**

### **O que é?**

A API de Países do IBGE é um **serviço público gratuito** mantido pelo Instituto Brasileiro de Geografia e Estatística que fornece informações completas e atualizadas sobre todos os países do mundo. Sem necessidade de cadastro, chaves ou autenticação!

### **URL da API:**

```
https://servicodados.ibge.gov.br/api/v1/paises
```

### **Exemplo de Resposta (1 país):**

```json
{
  "id": {
    "M49": 76,
    "ISO-3166-1-ALPHA-2": "BR",
    "ISO-3166-1-ALPHA-3": "BRA"
  },
  "nome": {
    "abreviado": "Brasil",
    "abreviado-EN": "Brazil",
    "abreviado-ES": "Brasil"
  },
  "area": {
    "total": "8515767",
    "unidade": {
      "nome": "quilômetros quadrados",
      "símbolo": "km2"
    }
  },
  "localizacao": {
    "regiao": {
      "id": { "M49": 19 },
      "nome": "América"
    },
    "sub-regiao": {
      "id": { "M49": 5 },
      "nome": "América do Sul"
    }
  },
  "governo": {
    "capital": {
      "nome": "Brasília"
    }
  },
  "linguas": [
    {
      "id": { "ISO-639-1": "pt" },
      "nome": "português"
    }
  ]
}
```

### **Por que esta API é perfeita para aprender?**

- **Gratuita**: Sem limites ou custos
- **Sem autenticação**: Não precisa de API keys
- **Dados oficiais**: Mantida pelo IBGE
- **CORS habilitado**: Funciona no navegador
- **Em português**: Dados em PT-BR
- **Confiável**: Serviço governamental estável

---

## **2. Nossa Primeira Requisição HTTP**

### **Passo 1: Testando no Navegador**

Antes de integrar no React, vamos ver a API funcionando:

1. Abra o navegador
2. Cole na barra de endereços: `https://servicodados.ibge.gov.br/api/v1/paises`
3. Pressione Enter

Você verá um **array gigante de objetos JSON** com dados de todos os países! 

### **Passo 2: Testando no Console do Navegador**

Abra o DevTools (F12) e cole no Console:

```javascript
fetch('https://servicodados.ibge.gov.br/api/v1/paises')
  .then(response => response.json())
  .then(data => {
    console.log('Total de países:', data.length);
    console.log('Primeiro país:', data[0]);
    console.log('Nome:', data[0].nome.abreviado);
    console.log('Capital:', data[0].governo.capital.nome);
  });
```

**Resultado esperado:**
```
Total de países: 250+
Primeiro país: {id: {...}, nome: {...}, ...}
Nome: Andorra
Capital: Andorra-a-Velha
```

---

## **3. Integrando no React - Versão Simples**

Vamos começar com a versão mais simples possível - um botão que carrega os países:

```jsx
// src/App.jsx
import { useState } from 'react';
import CountryGrid from './components/CountryGrid';
import './App.css';

function App() {
  const [countries, setCountries] = useState([]);
  const [favorites, setFavorites] = useState([]);

  // Função para buscar países da API do IBGE
  const loadCountries = () => {
    fetch('https://servicodados.ibge.gov.br/api/v1/paises')
      .then(response => response.json())
      .then(data => {
        console.log('Países carregados:', data.length);
        // Mapeia os dados do IBGE para o formato esperado pelo app
        const mapped = data.map(pais => ({
          cca3: pais.id['ISO-3166-1-ALPHA-3'],
          flag: `https://flagcdn.com/${pais.id['ISO-3166-1-ALPHA-2'].toLowerCase()}.svg`,
          name: pais.nome.abreviado,
          capital: pais.governo?.capital?.nome || 'N/A',
          population: 0, // API do IBGE não retorna população
          region: pais.localizacao.regiao.nome,
          subregion: pais.localizacao['sub-regiao']?.nome || 'N/A'
        }));
        setCountries(mapped);
      })
      .catch(error => {
        console.error('Erro ao carregar países:', error);
      });
  };

  const toggleFavorite = (countryCode) => {
    setFavorites(prev => 
      prev.includes(countryCode)
        ? prev.filter(code => code !== countryCode)
        : [...prev, countryCode]
    );
  };

  return (
    <div className="app">
      <header className="app-header">
        <h1>Lista de Países do Mundo</h1>
        <p>Dados reais da API do IBGE</p>
        
        {/* Botão para carregar */}
        <button onClick={loadCountries} className="load-btn">
          Carregar Países da API do IBGE
        </button>
        
        {countries.length > 0 && (
          <p className="loaded-info">
            {countries.length} países carregados!
          </p>
        )}
      </header>

      {countries.length > 0 ? (
        <CountryGrid 
          countries={countries}
          favorites={favorites}
          onToggleFavorite={toggleFavorite}
        />
      ) : (
        <div className="placeholder">
          <p>Clique no botão acima para carregar os países</p>
        </div>
      )}
    </div>
  );
}

export default App;
```

**Importante!** Note que estamos **mapeando** os dados da API do IBGE para o formato que nossos componentes esperam. A estrutura do IBGE é diferente, então criamos um objeto com os campos que precisamos:

- `cca3`: código ISO-3166-1-ALPHA-3 (BRA, USA, etc.)
- `flag`: montamos a URL da bandeira usando o código ALPHA-2
- `name`: nome abreviado do país em português
- `capital`: extraído do objeto governo
- `region` e `subregion`: vêm da localização

**Teste agora!** Clique no botão e veja os dados reais carregando!

---

## **4. Adicionando Estados de Loading e Erro**

Agora vamos melhorar com feedback visual durante o carregamento:

```jsx
// src/App.jsx - Versão com Loading e Erro
import { useState } from 'react';
import CountryGrid from './components/CountryGrid';
import Loading from './components/Loading';
import ErrorMessage from './components/ErrorMessage';
import './App.css';

function App() {
  const [countries, setCountries] = useState([]);
  const [favorites, setFavorites] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  const loadCountries = async () => {
    setIsLoading(true);
    setError(null);
    
    try {
      console.log('Iniciando requisição para API do IBGE...');
      
      const response = await fetch('https://servicodados.ibge.gov.br/api/v1/paises');
      
      console.log('Resposta recebida, status:', response.status);
      
      if (!response.ok) {
        throw new Error(`Erro HTTP: ${response.status}`);
      }
      
      const data = await response.json();
      
      console.log('Dados processados:', data.length, 'países');
      
      // Mapeia os dados do IBGE para o formato do app
      const mapped = data.map(pais => ({
        cca3: pais.id['ISO-3166-1-ALPHA-3'],
        flag: `https://flagcdn.com/${pais.id['ISO-3166-1-ALPHA-2'].toLowerCase()}.svg`,
        name: pais.nome.abreviado,
        capital: pais.governo?.capital?.nome || 'N/A',
        population: 0, // API do IBGE não retorna população
        region: pais.localizacao.regiao.nome,
        subregion: pais.localizacao['sub-regiao']?.nome || 'N/A'
      }));
      
      setCountries(mapped);
      
    } catch (err) {
      console.error('Erro:', err);
      setError(err.message);
    } finally {
      setIsLoading(false);
    }
  };

  const toggleFavorite = (countryCode) => {
    setFavorites(prev => 
      prev.includes(countryCode)
        ? prev.filter(code => code !== countryCode)
        : [...prev, countryCode]
    );
  };

  return (
    <div className="app">
      <header className="app-header">
        <h1>Lista de Países do Mundo</h1>
        <p>Conectado à API do IBGE</p>
        
        <button 
          onClick={loadCountries} 
          disabled={isLoading}
          className="load-btn"
        >
          {isLoading ? 'Carregando...' : 'Carregar Países'}
        </button>
        
        {countries.length > 0 && (
          <div className="header-stats">
            <span>{countries.length} países</span>
            <span>{favorites.length} favoritos</span>
          </div>
        )}
      </header>

      <main className="main-content">
        {isLoading && <Loading />}
        
        {error && (
          <ErrorMessage 
            message={error}
            onRetry={loadCountries}
          />
        )}
        
        {!isLoading && !error && countries.length > 0 && (
          <CountryGrid 
            countries={countries}
            favorites={favorites}
            onToggleFavorite={toggleFavorite}
          />
        )}
        
        {!isLoading && !error && countries.length === 0 && (
          <div className="welcome-state">
            <h2>Bem-vindo!</h2>
            <p>Clique no botão acima para carregar dados de todos os países do mundo.</p>
          </div>
        )}
      </main>
    </div>
  );
}

export default App;
```

---

## **5. Componentes de Feedback Visual**

### **Loading Component**

```jsx
// src/components/Loading.jsx
function Loading() {
  return (
    <div className="loading-container">
      <div className="loading-spinner"></div>
      <h2>Carregando países...</h2>
      <p>Buscando dados de 250+ países da API do IBGE</p>
    </div>
  );
}

export default Loading;
```

### **ErrorMessage Component**

```jsx
// src/components/ErrorMessage.jsx
function ErrorMessage({ message, onRetry }) {
  return (
    <div className="error-container">
      <div className="error-icon">❌</div>
      <h2>Ops! Algo deu errado</h2>
      <p className="error-message">{message}</p>
      <button onClick={onRetry} className="retry-btn">
        Tentar Novamente
      </button>
      <div className="error-tips">
        <p><strong>💡 Possíveis causas:</strong></p>
        <ul>
          <li>Verifique sua conexão com a internet</li>
          <li>A API pode estar temporariamente indisponível</li>
          <li>Firewall ou proxy bloqueando a requisição</li>
        </ul>
      </div>
    </div>
  );
}

export default ErrorMessage;
```

---

## **6. Adaptando Componentes para Dados Reais**

Os dados da API do IBGE têm estrutura diferente. Já fizemos o mapeamento no `App.jsx`, mas vamos entender melhor os componentes:

### **Estrutura dos Dados Mapeados**

Após o mapeamento, cada país tem:
```javascript
{
  cca3: "BRA",              // Código ISO de 3 letras
  flag: "https://...",      // URL da bandeira
  name: "Brasil",           // Nome em português
  capital: "Brasília",      // Capital
  population: 0,            // Não disponível no IBGE
  region: "América",        // Região
  subregion: "América do Sul" // Sub-região
}
```

### **CountryCard Adaptado**

```jsx
// src/components/CountryCard.jsx
function CountryCard({ 
  cca3, 
  flag, 
  name, 
  capital, 
  region,
  subregion,
  isFavorite, 
  onToggleFavorite 
}) {
  const formatPopulation = (pop) => {
    if (!pop || pop === 0) return 'Não disponível';
    return new Intl.NumberFormat('pt-BR').format(pop);
  };

  return (
    <div className={`country-card ${isFavorite ? 'favorite' : ''}`}>
      <div className="country-header">
        <img 
          src={flag} 
          alt={`Bandeira de ${name}`}
          className="flag-img"
          onError={(e) => {
            e.target.onerror = null;
            e.target.src = `https://via.placeholder.com/80x60?text=${name.charAt(0)}`;
          }}
        />
        <h3>{name}</h3>
      </div>

      <div className="country-info">
        <div className="info-row">
          <span className="label">Capital:</span>
          <span className="value">{capital}</span>
        </div>
        {/*<div className="info-row">
          <span className="label">População:</span>
          <span className="value">{formatPopulation(population)}</span>
        </div>
        */}
        <div className="info-row">
          <span className="label">Região:</span>
          <span className="value">{region}</span>
        </div>
        <div className="info-row">
          <span className="label">Sub-região:</span>
          <span className="value">{subregion}</span>
        </div>
      </div>

      <button 
        className={`favorite-btn ${isFavorite ? 'favorited' : ''}`}
        onClick={() => onToggleFavorite(cca3)}
        title={isFavorite ? 'Remover dos favoritos' : 'Adicionar aos favoritos'}
      >
        {isFavorite ? '❤️ Favorito' : '🤍 Favoritar'}
      </button>
    </div>
  );
}

export default CountryCard;
```

**Mudanças importantes:**
- ✅ Substituímos emoji da bandeira por `<img>` com URL da flagcdn (formato SVG)
- ✅ Adicionamos `onError` para fallback se a bandeira não carregar (usa placeholder com primeira letra)
- ✅ Tratamos população como "Não disponível" quando não houver dados
- ✅ Usamos `info-row` com labels e valores para layout organizado

### **CountryGrid Adaptado**

```jsx
// src/components/CountryGrid.jsx
import CountryCard from './CountryCard';

function CountryGrid({ countries, favorites, onToggleFavorite }) {
  return (
    <div className="country-grid">
      {countries.map((country) => (
        <CountryCard 
          key={country.cca3}
          cca3={country.cca3}
          flag={country.flag}
          name={country.name}
          capital={country.capital}
          population={country.population}
          region={country.region}
          subregion={country.subregion}
          isFavorite={favorites.includes(country.cca3)}
          onToggleFavorite={onToggleFavorite}
        />
      ))}
    </div>
  );
}

export default CountryGrid;
```

**Como funciona:**
- Recebe o array `countries` já mapeado do App
- Faz `.map()` para criar um `CountryCard` para cada país
- Passa cada propriedade individualmente como prop (spread manual)
- Calcula `isFavorite` verificando se o `cca3` está no array de favoritos
- Usa `country.cca3` como `key` para otimização do React

---

## **7. Estilizando os Estados Visuais**

```css
/* src/App.css - Adições para Loading e Erro */

/* Loading State */
.loading-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 4rem 2rem;
  text-align: center;
}

.loading-spinner {
  width: 60px;
  height: 60px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 1.5rem;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.loading-container h2 {
  color: #2c3e50;
  margin-bottom: 0.5rem;
}

.loading-container p {
  color: #7f8c8d;
  font-size: 0.95rem;
}

/* Error State */
.error-container {
  max-width: 500px;
  margin: 2rem auto;
  padding: 2rem;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  text-align: center;
}

.error-icon {
  font-size: 4rem;
  margin-bottom: 1rem;
}

.error-container h2 {
  color: #e74c3c;
  margin-bottom: 1rem;
}

.error-message {
  color: #7f8c8d;
  background: #fef5f5;
  padding: 1rem;
  border-radius: 8px;
  border-left: 4px solid #e74c3c;
  margin-bottom: 1.5rem;
  font-family: monospace;
}

.retry-btn {
  background: #3498db;
  color: white;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.3s;
  margin-bottom: 1.5rem;
}

.retry-btn:hover {
  background: #2980b9;
  transform: translateY(-2px);
}

.error-tips {
  background: #f8f9fa;
  padding: 1rem;
  border-radius: 8px;
  text-align: left;
  margin-top: 1rem;
}

.error-tips p {
  font-weight: bold;
  margin-bottom: 0.5rem;
  color: #34495e;
}

.error-tips ul {
  list-style: none;
  padding: 0;
}

.error-tips li {
  padding: 0.4rem 0;
  color: #7f8c8d;
  font-size: 0.9rem;
}

.error-tips li:before {
  content: "→ ";
  color: #3498db;
  font-weight: bold;
}

/* Welcome/Empty State */
.welcome-state {
  text-align: center;
  padding: 4rem 2rem;
  color: #7f8c8d;
}

.welcome-state h2 {
  color: #2c3e50;
  margin-bottom: 1rem;
}

/* Header Stats */
.header-stats {
  display: flex;
  gap: 1.5rem;
  justify-content: center;
  margin-top: 1rem;
  font-size: 0.95rem;
}

.header-stats span {
  background: white;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

/* Load Button */
.load-btn {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  padding: 0.875rem 2rem;
  border-radius: 12px;
  font-size: 1.05rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
  margin-top: 1rem;
}

.load-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(102, 126, 234, 0.4);
}

.load-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.loaded-info {
  margin-top: 1rem;
  padding: 0.75rem 1.5rem;
  background: #d4edda;
  color: #155724;
  border-radius: 8px;
  font-weight: 500;
}

/* Bandeiras (imagens) */
.flag-img {
  width: 60px;
  height: auto;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  object-fit: cover;
}

.country-header {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1rem;
  padding-bottom: 0.75rem;
  border-bottom: 1px solid #eee;
}
```

---

## **Exercícios Práticos**

### **Exercício 1: Teste no Console**
Abra o DevTools e execute:
```javascript
fetch('https://servicodados.ibge.gov.br/api/v1/paises')
  .then(r => r.json())
  .then(d => {
    const brasil = d.find(p => p.id['ISO-3166-1-ALPHA-3'] === 'BRA');
    console.log('Brasil:', brasil.nome.abreviado);
    console.log('Capital:', brasil.governo.capital.nome);
    console.log('Região:', brasil.localizacao.regiao.nome);
  });
```

### **Exercício 2: Adicione um Contador**
Mostre quantos países foram carregados em tempo real durante o fetch.

### **Exercício 3: Filtre por Região**
Adicione botões que filtram países por região (América, Europa, Ásia, África, Oceania).

### **Exercício 4: Mensagens Personalizadas**
Modifique `ErrorMessage` para mostrar dicas diferentes dependendo do tipo de erro (rede, servidor, timeout).

### **Exercício 5: Indicador de Velocidade**
Adicione um timer que mostra quanto tempo levou para carregar os dados:
```jsx
const [loadTime, setLoadTime] = useState(null);
// Calcule o tempo entre início e fim do fetch
```

### **Exercício 6: Cache Local**
Salve os países no `localStorage` após carregar e use como fallback se a API estiver indisponível.

### **Exercício 7: Exibir Mais Informações**
A API do IBGE retorna mais dados (área, idiomas, moedas). Modifique o mapeamento para incluir esses campos e exiba-os no card.

### **Exercício 8: Comparar APIs**
Compare os dados do IBGE com outra API (como REST Countries) e veja as diferenças na estrutura e informações disponíveis.
