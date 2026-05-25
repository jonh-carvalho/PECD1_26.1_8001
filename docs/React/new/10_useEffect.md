---
id: 10_useEffect
title: 10 - useEffect - Carregamento Automático
---

# 10 - useEffect - Carregamento Automático

Neste módulo, vamos automatizar o carregamento da API quando a tela abre. O foco é entender side effects e dependências do useEffect.

## Objetivos do Módulo

- Carregar dados automaticamente na montagem do componente
- Entender useEffect com array de dependências
- Separar loading, erro e sucesso
- Reaproveitar o mapeamento de dados da API

## Pré-requisitos

- Consumo de API com fetch
- Noções de Promise, async/await e try/catch/finally
- useState para estados da tela
- Estrutura da Lista de Países

## Conceito Principal

useEffect executa efeitos colaterais após a renderização. Com array vazio, roda uma vez na montagem, ideal para buscar dados iniciais.

## Conexão com o Módulo 09

No módulo 09, a requisição era disparada por botão. Agora a mesma lógica vai rodar automaticamente ao abrir a tela.

Regra importante: o callback do `useEffect` não deve ser `async` diretamente. O padrão é criar uma função `async` interna e chamá-la dentro do efeito.

## Exemplo Guiado

```jsx
import { useEffect, useState } from 'react';

function App() {
  const [countries, setCountries] = useState([]);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const loadCountries = async () => {
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

    loadCountries();
  }, []);

  if (isLoading) return <p>Carregando países...</p>;
  if (error) return <p>Erro: {error}</p>;

  return (
    <div>
      <h1>Lista de Países</h1>
      <p>Total carregado: {countries.length}</p>
    </div>
  );
}

export default App;
```

## Exercício Prático

1. Mostre mensagem diferente para loading, erro e sucesso.
2. Crie botão Tentar Novamente para recarregar em caso de erro.
3. Exiba o nome dos 5 primeiros países no estado de sucesso.

## Erros Comuns

- Esquecer o array de dependências e causar várias requisições
- Atualizar estado fora de try/catch/finally
- Misturar lógica de filtro dentro do mesmo efeito

## Próximo Passo

No próximo módulo você vai aplicar filtros básicos sobre os dados carregados.
