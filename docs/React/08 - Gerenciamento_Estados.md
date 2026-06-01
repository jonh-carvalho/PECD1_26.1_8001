---
id: 08_Gerenciamento_de_Estados
title: 08 - Gerenciamento de Estados
---

# 08 - Gerenciamento de Estados

Neste módulo, o foco é controlar vários estados ao mesmo tempo e entender estado derivado. Sem filtros, sem busca, sem persistência — apenas a consolidação de múltiplos estados na mesma tela.

## Objetivos do Módulo

- Gerenciar múltiplos estados independentes na mesma tela
- Trabalhar com estado derivado (calculado a partir de outros estados)
- Organizar o estado no componente pai e distribuir para filhos por props
- Evitar duplicação de dados criando estados desnecessários

## Pré-requisitos

- JSX e componentes funcionais
- Props e comunicação entre componentes
- useState e eventos básicos

## Conceito Principal

Quando a aplicação cresce, cada estado deve ter uma responsabilidade clara. O maior erro é criar estado para valores que podem ser calculados (estado derivado). Por exemplo: **não crie um estado para "favoriteCount", calcule a partir do array de favoritos**.

## Exemplo Guiado

```jsx
import { useState } from 'react';

function App() {
  // Dados imutáveis (não mudam neste módulo)
  const [countries] = useState([
    { id: 1, name: 'Brasil', flag: '🇧🇷', capital: 'Brasília' },
    { id: 2, name: 'Argentina', flag: '🇦🇷', capital: 'Buenos Aires' },
    { id: 3, name: 'Chile', flag: '🇨🇱', capital: 'Santiago' }
  ]);

  // Estados independentes
  const [favorites, setFavorites] = useState([]);
  const [showCapitals, setShowCapitals] = useState(true);

  // Função para alternar favoritos
  const toggleFavorite = (id) => {
    setFavorites((prev) =>
      prev.includes(id) ? prev.filter((item) => item !== id) : [...prev, id]
    );
  };

  // Estado derivado: NÃO criar novo estado, calcular quando necessário
  const favoriteCount = favorites.length;

  return (
    <div>
      <h1>Lista de Países</h1>
      <p>Total de favoritos: {favoriteCount}</p>

      <button onClick={() => setShowCapitals((prev) => !prev)}>
        {showCapitals ? 'Ocultar Capitais' : 'Mostrar Capitais'}
      </button>

      <ul>
        {countries.map((country) => (
          <li key={country.id}>
            {country.flag} {country.name}
            {showCapitals && ` - ${country.capital}`}
            <button onClick={() => toggleFavorite(country.id)}>
              {favorites.includes(country.id) ? '★' : '☆'} Favorito
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

1. Adicione um estado `showIds` para mostrar/ocultar IDs dos países.
2. Crie um botão "Limpar Favoritos" que reseta o array de favoritos.
3. Exiba quantos países têm capitais (sem criar novo estado — calcule na hora).

## Erros Comuns

- Criar estado para valores derivados (exemplo: salvar favoriteCount em useState)
- Atualizar array diretamente em vez de criar novo array
- Colocar estado em componente filho quando ele precisa ser compartilhado

## Nota Importante

Neste módulo, favoritos são **apenas uma lista de IDs favoritos** sem filtros. No módulo 11, vamos adicionar a capacidade de **filtrar por favoritos**. No módulo 12, vamos torná-los **persistentes com localStorage**. No módulo 13, vamos **consolidar tudo junto**.

## Próximo Passo

No próximo módulo você vai conectar a aplicação com uma API real, carregando dados de uma fonte externa e mantendo a mesma estrutura de estados.
