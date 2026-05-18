---
id: 08_Gerenciamento_de_Estados
title: 08 - Gerenciamento de Estados
---

# 08 - Gerenciamento de Estados

Neste módulo, o foco é controlar vários estados ao mesmo tempo sem misturar busca, filtros avançados ou persistência. A meta é consolidar o raciocínio de estado antes de entrar em dados externos.

## Objetivos do Módulo

- Gerenciar múltiplos estados relacionados na mesma tela
- Trabalhar com estado derivado
- Organizar o estado no componente pai e distribuir para filhos por props
- Implementar favoritos e contadores sem duplicar dados

## Pré-requisitos

- JSX e componentes funcionais
- Props e comunicação entre componentes
- useState e eventos básicos

## Conceito Principal

Quando a aplicação cresce, manter um estado por funcionalidade ajuda a evitar bugs. Também é importante calcular valores derivados, como quantidade de favoritos, sem criar um novo estado desnecessário.

## Exemplo Guiado

```jsx
import { useState } from 'react';

function App() {
  const [countries] = useState([
    { id: 1, name: 'Brasil', flag: '🇧🇷', capital: 'Brasília' },
    { id: 2, name: 'Argentina', flag: '🇦🇷', capital: 'Buenos Aires' },
    { id: 3, name: 'Chile', flag: '🇨🇱', capital: 'Santiago' }
  ]);
  const [favorites, setFavorites] = useState([]);
  const [showOnlyFavorites, setShowOnlyFavorites] = useState(false);

  const toggleFavorite = (id) => {
    setFavorites((prev) =>
      prev.includes(id) ? prev.filter((item) => item !== id) : [...prev, id]
    );
  };

  const favoriteCount = favorites.length;
  const visibleCountries = showOnlyFavorites
    ? countries.filter((country) => favorites.includes(country.id))
    : countries;

  return (
    <div>
      <h1>Lista de Países</h1>
      <p>Favoritos: {favoriteCount}</p>

      <button onClick={() => setShowOnlyFavorites((prev) => !prev)}>
        {showOnlyFavorites ? 'Mostrar Todos' : 'Mostrar Favoritos'}
      </button>

      <ul>
        {visibleCountries.map((country) => (
          <li key={country.id}>
            {country.flag} {country.name} - {country.capital}
            <button onClick={() => toggleFavorite(country.id)}>
              {favorites.includes(country.id) ? 'Remover Favorito' : 'Favoritar'}
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

## Exercício Prático

1. Adicione um estado para mostrar ou ocultar a capital dos países.
2. Exiba um contador com o total de países visíveis.
3. Crie um botão para limpar todos os favoritos.

## Erros Comuns

- Criar estado para valores derivados, por exemplo salvar favoriteCount em useState
- Atualizar array diretamente em vez de criar novo array
- Colocar estado em componente filho quando ele precisa ser compartilhado

## Próximo Passo

No próximo módulo você vai conectar a aplicação com uma API real, mantendo a mesma estrutura de estados para receber dados externos.
