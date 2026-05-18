---
id: 09_Conectando_com_APIs_Reais
title: 09 - Conectando com APIs Reais
---

# 09 - Conectando com APIs Reais

Neste módulo, vamos sair dos dados locais e carregar informações reais da web usando fetch. Para manter consistência da trilha, usamos somente a REST Countries API.

## Objetivos do Módulo

- Fazer requisição HTTP com fetch
- Ler e mapear dados da REST Countries API
- Tratar loading e erro
- Adaptar o formato da API para o formato da aplicação

## Pré-requisitos

- useState e eventos
- Arrays e métodos map e filter
- Estrutura básica da Lista de Países

## Conceito Principal

APIs reais retornam dados em formato diferente do seu componente. O mapeamento é a etapa que transforma a resposta da API no formato usado pela interface.

## Exemplo Guiado

```jsx
import { useState } from 'react';

function App() {
  const [countries, setCountries] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  const loadCountries = async () => {
    setIsLoading(true);
    setError(null);

    try {
      const response = await fetch('https://restcountries.com/v3.1/all');
      if (!response.ok) throw new Error('Falha ao carregar países');

      const data = await response.json();
      const mapped = data.map((country) => ({
        cca3: country.cca3,
        name: country.name.common,
        capital: country.capital?.[0] || 'N/A',
        region: country.region,
        flag: country.flag
      }));

      setCountries(mapped);
    } catch (err) {
      setError(err.message);
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div>
      <h1>API Real</h1>
      <button onClick={loadCountries}>Carregar Países</button>
      {isLoading && <p>Carregando...</p>}
      {error && <p>Erro: {error}</p>}
      <p>Total: {countries.length}</p>
    </div>
  );
}

export default App;
```

## Exercício Prático

1. Exiba os 10 primeiros países carregados.
2. Mostre nome, capital e região de cada país.
3. Crie um botão para recarregar os dados.

## Erros Comuns

- Esquecer de validar response.ok
- Tentar usar campos da API sem verificar se existem
- Não limpar erro antes de nova requisição

## Próximo Passo

No próximo módulo, o carregamento será automático com useEffect, sem precisar de botão.
