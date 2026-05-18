---
id: 03_JSX_Sintaxe
title: 03 - JSX e Sintaxe do React
---
# 03 - **JSX e Sintaxe do React**

## Introdução

**JSX** (JavaScript XML) é uma extensão de sintaxe para JavaScript que permite escrever código que parece HTML dentro do JavaScript. É a forma padrão de escrever componentes React, tornando o código mais legível e intuitivo para desenvolvedores que já conhecem HTML.

**Por que usar JSX?**
- Sintaxe familiar e intuitiva (similar ao HTML)
- Melhor legibilidade do código
- Facilita a criação de interfaces de usuário
- Permite a mistura de lógica JavaScript com marcação HTML

### Características principais do JSX

1\. **Sintaxe familiar**:
- Parece HTML, mas é na verdade JavaScript transformado
- Permite usar tags e atributos similares ao HTML

2\. **Expressões JavaScript**:
- Permite inserir expressões JavaScript usando chaves `{}`
- Pode incluir variáveis, funções e operações

3\. **Componentes como elementos**:
- Componentes React podem ser usados como elementos JSX
- Facilita a composição e reutilização de código

4\. **Compilação**:
- JSX é transformado em chamadas `React.createElement()` durante o build
- Ferramentas como Vite fazem essa transformação automaticamente

### Sintaxe Básica do JSX

**HTML vs JSX:**

```jsx
// HTML tradicional
<div class="container">
  <h1>Olá Mundo</h1>
  <p>Este é um parágrafo</p>
</div>

// JSX (React)
<div className="container">
  <h1>Olá Mundo</h1>
  <p>Este é um parágrafo</p>
</div>
```

**Principais diferenças:**
- `class` → `className`
- `for` → `htmlFor`
- Atributos em camelCase: `onClick`, `onChange`
- Tags devem ser fechadas: `<img />`, `<br />`

### Expressões JavaScript no JSX

```jsx
function Welcome() {
  const name = "React";
  const isLoggedIn = true;

  return (
    <div>
      <h1>Olá, {name}!</h1>
      <p>Você está {isLoggedIn ? 'logado' : 'deslogado'}</p>
      <p>Resultado: {2 + 2}</p>
    </div>
  );
}
```

### Renderização Condicional

```jsx
function UserGreeting({ user }) {
  return (
    <div>
      {user ? (
        <h1>Bem-vindo, {user.name}!</h1>
      ) : (
        <h1>Por favor, faça login</h1>
      )}
    </div>
  );
}
```

### Listas em JSX

```jsx
function CountryList() {
  const countries = ['Brasil', 'Argentina', 'Chile', 'Uruguai'];

  return (
    <ul>
      {countries.map((country, index) => (
        <li key={index}>{country}</li>
      ))}
    </ul>
  );
}
```

### Iniciando o Projeto: Lista de Países

Vamos começar a construir um projeto que evolui ao longo do curso. Começaremos com uma lista simples de países que será expandida nos próximos módulos.

**Projeto Atual: Lista Estática**

```jsx
function App() {
  return (
    <div className="app">
      <h1>🌍 Lista de Países</h1>
      <div className="country-grid">
        <div className="country-card">
          <h3>🇧🇷 Brasil</h3>
          <p>Capital: Brasília</p>
        </div>
        <div className="country-card">
          <h3>🇦🇷 Argentina</h3>
          <p>Capital: Buenos Aires</p>
        </div>
        <div className="country-card">
          <h3>🇨🇱 Chile</h3>
          <p>Capital: Santiago</p>
        </div>
      </div>
    </div>
  );
}

export default App;
```

**CSS básico (App.css):**

```css
.app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.country-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

.country-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 15px;
  background-color: #f9f9f9;
}

.country-card h3 {
  margin: 0 0 10px 0;
  color: #333;
}

.country-card p {
  margin: 5px 0;
  color: #666;
}
```

### Próximos Passos do Projeto

Ao longo do curso, este projeto evoluirá:

- **Módulo 4**: Separar em componentes reutilizáveis
- **Módulo 5**: Usar props para passar dados
- **Módulo 6**: Adicionar interatividade com useState
- **Módulo 8**: Renderizar listas dinamicamente
- **Módulo 11**: Carregar dados de arquivo JSON
- **Módulo 12**: Consumir API REST Countries
- **Módulo 13**: Adicionar filtros e busca

### Exercícios Práticos

**1. Modificar o projeto:**
- Adicione mais países à lista
- Inclua informações como população ou idioma
- Experimente com diferentes emojis de bandeiras

**2. Praticar JSX:**
- Crie um componente que mostra a data e hora atual
- Use expressões JavaScript para calcular a idade de uma pessoa
- Implemente renderização condicional para mostrar/ocultar conteúdo

**3. Explorar sintaxe:**
- Experimente diferentes atributos HTML em JSX
- Pratique o uso de chaves `{}` para inserir JavaScript
- Teste renderização de listas com diferentes tipos de dados

### Conceitos Importantes

- **JSX é obrigatório?** Não, mas é a forma padrão e recomendada
- **Diferenças do HTML:** Alguns atributos mudam (class → className)
- **JavaScript no JSX:** Use chaves `{}` para inserir código JavaScript
- **Componentes:** JSX permite usar componentes como elementos HTML
- **Compilação:** JSX é transformado em JavaScript durante o build

### Próximo Módulo

No próximo módulo, aprenderemos sobre **Componentes**, onde dividiremos nosso projeto em partes menores e reutilizáveis, aplicando os conceitos de JSX que acabamos de aprender.
