---
id: 12_Favoritos_localStorage
title: 12 - Favoritos e Persistência com localStorage
---

# 12 - Favoritos e Persistência com localStorage

Neste módulo, o foco é salvar e restaurar favoritos no navegador. A lógica de busca e filtro permanece simples e não será expandida aqui.

## Objetivos do Módulo

- Salvar favoritos no localStorage
- Restaurar favoritos na inicialização da aplicação
- Sincronizar estado React com armazenamento local
- Evitar loops de atualização com useEffect

## Pré-requisitos

- Estados com useState
- useEffect para efeitos colaterais
- Lista de países carregada da API

## Conceito Principal

localStorage salva dados em formato string. Para armazenar arrays ou objetos, é necessário converter com JSON.stringify e recuperar com JSON.parse.

## Exemplo Guiado

```jsx
import { useEffect, useState } from 'react';

function App() {
  const [favorites, setFavorites] = useState([]);

  useEffect(() => {
    const stored = localStorage.getItem('countryFavorites');
    if (stored) {
      try {
        setFavorites(JSON.parse(stored));
      } catch {
        localStorage.removeItem('countryFavorites');
      }
    }
  }, []);

  useEffect(() => {
    localStorage.setItem('countryFavorites', JSON.stringify(favorites));
  }, [favorites]);

  const toggleFavorite = (code) => {
    setFavorites((prev) =>
      prev.includes(code)
        ? prev.filter((item) => item !== code)
        : [...prev, code]
    );
  };

  return (
    <div>
      <h1>Favoritos Persistentes</h1>
      <p>Total de favoritos: {favorites.length}</p>

      <button onClick={() => toggleFavorite('BRA')}>Alternar Brasil</button>
      <button onClick={() => toggleFavorite('ARG')}>Alternar Argentina</button>
      <button onClick={() => setFavorites([])}>Limpar Favoritos</button>
    </div>
  );
}

export default App;
```

## Exercício Prático

1. Crie um botão para exportar os favoritos no console.
2. Adicione confirmação antes de limpar todos os favoritos.
3. Mostre uma mensagem quando não houver favoritos salvos.

## Erros Comuns

- Tentar salvar array sem JSON.stringify
- Chamar JSON.parse sem tratar erro
- Misturar leitura e escrita do localStorage no mesmo efeito com dependência de favorites

## Próximo Passo

No próximo módulo você vai consolidar busca e filtros da aplicação com foco em experiência final de uso.
