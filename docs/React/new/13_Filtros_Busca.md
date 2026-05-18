---
id: 13_Filtros_Busca
title: 13 - Consolidação de Filtros e Busca
---

# 13 - Consolidação de Filtros e Busca

Este módulo fecha a etapa de listagem da aplicação. O foco é combinar busca, região e favoritos em um fluxo claro de uso, sem avançar para funcionalidades excessivamente complexas.

## Objetivos do Módulo

- Consolidar busca textual, filtro por região e favoritos
- Melhorar a experiência de uso com feedback de resultados
- Organizar componentes de filtro para facilitar manutenção
- Preparar a base para rotas no próximo módulo

## Pré-requisitos

- Filtros básicos por nome e região
- Favoritos persistidos com localStorage
- Carregamento de dados com useEffect

## Conceito Principal

Consolidar filtros significa aplicar critérios em sequência, mantendo previsibilidade: primeiro favoritos, depois região, depois busca textual.

## Exemplo Guiado

```jsx
const filteredCountries = countries
  .filter((country) => {
    if (!showOnlyFavorites) return true;
    return favorites.includes(country.cca3);
  })
  .filter((country) => {
    if (selectedRegion === 'all') return true;
    return country.region === selectedRegion;
  })
  .filter((country) => {
    const search = searchTerm.trim().toLowerCase();
    if (!search) return true;
    return country.name.common.toLowerCase().includes(search);
  });
```

```jsx
return (
  <div>
    <h1>Lista de Países</h1>

    <div className="filters">
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

      <button onClick={() => setShowOnlyFavorites((prev) => !prev)}>
        {showOnlyFavorites ? 'Mostrar Todos' : 'Somente Favoritos'}
      </button>

      <button
        onClick={() => {
          setSearchTerm('');
          setSelectedRegion('all');
          setShowOnlyFavorites(false);
        }}
      >
        Limpar Filtros
      </button>
    </div>

    <p>Resultados: {filteredCountries.length}</p>
  </div>
);
```

## Exercício Prático

1. Adicione ordenação por nome em ordem alfabética.
2. Mostre um estado vazio amigável quando não houver resultados.
3. Exiba no topo quais filtros estão ativos.

## Erros Comuns

- Aplicar filtros em ordem confusa e obter resultados inconsistentes
- Não limpar filtros de forma centralizada
- Misturar lógica de busca avançada prematuramente

## Próximo Passo

No próximo módulo você vai estruturar navegação com React Router, criando páginas e detalhes por país.
