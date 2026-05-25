---
id: 11_Filtros_Basicos
title: 11 - Filtros Básicos
---

# 11 - Filtros Básicos

Até agora tínhamos todos os países visíveis. Neste módulo, o foco é implementar a **primeira camada de filtros**: busca por nome em tempo real e filtro por região. Persistência (localStorage) e consolidação avançada ficam para módulos posteriores.

## Objetivos do Módulo

- Implementar busca por texto em tempo real
- Implementar filtro por região
- Combinar busca e região em uma única filtragem
- Mostrar quantidade de resultados de forma clara

## Pré-requisitos

- useEffect para carregamento de dados
- useState para estado local
- map e filter em arrays

## Conceito Principal

Filtros básicos são regras de exibição. O dado original permanece inteiro, e a interface mostra apenas o subconjunto correspondente aos critérios ativos.

## Exemplo Guiado

```jsx
import { useEffect, useState } from 'react';

function App() {
  const [countries, setCountries] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');
  const [selectedRegion, setSelectedRegion] = useState('all');
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const loadCountries = async () => {
      try {
        const response = await fetch('https://restcountries.com/v3.1/all');
        if (!response.ok) throw new Error('Falha ao carregar dados');
        const data = await response.json();
        setCountries(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setIsLoading(false);
      }
    };

    loadCountries();
  }, []);

  const filteredCountries = countries.filter((country) => {
    const name = country.name.common.toLowerCase();
    const matchesSearch = name.includes(searchTerm.toLowerCase());
    const matchesRegion =
      selectedRegion === 'all' || country.region === selectedRegion;
    return matchesSearch && matchesRegion;
  });

  if (isLoading) return <p>Carregando...</p>;
  if (error) return <p>Erro: {error}</p>;

  return (
    <div>
      <h1>Filtros Básicos</h1>

      <input
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Buscar país"
      />

      <select
        value={selectedRegion}
        onChange={(e) => setSelectedRegion(e.target.value)}
      >
        <option value="all">Todas as regiões</option>
        <option value="Africa">África</option>
        <option value="Americas">Américas</option>
        <option value="Asia">Ásia</option>
        <option value="Europe">Europa</option>
        <option value="Oceania">Oceania</option>
      </select>

      <p>Resultados: {filteredCountries.length}</p>

      <ul>
        {filteredCountries.map((country) => (
          <li key={country.cca3}>{country.name.common}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

## Exercício Prático

1. Adicione um botão para limpar busca e região.
2. Mostre uma mensagem quando nenhum país for encontrado.
3. Exiba no cabeçalho o filtro ativo.

## Erros Comuns

- Filtrar a lista original e sobrescrever countries
- Aplicar filtro de região com valores diferentes dos retornados pela API
- Esquecer de tratar estado de loading e erro

## Próximo Passo

No próximo módulo você vai persistir favoritos com localStorage, mantendo os filtros deste módulo.
