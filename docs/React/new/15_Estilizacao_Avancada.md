---
id: 15_Estilizacao_Avancada
title: 15 - Estilização Avançada
---

# 15 - Estilização Avançada

Este módulo encerra a trilha com foco em acabamento visual, consistência e responsividade da **Lista de Países**. A lógica principal da aplicação já está pronta; agora a meta é melhorar experiência e apresentação do projeto final de portfólio.

## Objetivos do Módulo

- Organizar estilos globais e por componente da Lista de Países
- Definir paleta, espaçamento e tipografia consistentes
- Melhorar responsividade para grade de países, filtros e navegação
- Aplicar estados visuais para loading, erro, favoritos e links ativos

## Pré-requisitos

- Aplicação com rotas e listagem funcionando
- Componentes separados por responsabilidade
- CSS básico ou CSS Modules

## Conceito Principal

Estilização avançada não é só aparência. Ela melhora legibilidade, navegação e previsibilidade de uso.

Na prática, vamos padronizar elementos que já existem no projeto: cabeçalho, barra de filtros, cards de país, botões de favorito e estados de feedback.

## Exemplo Guiado

```css
:root {
  --bg: #f7f9fc;
  --surface: #ffffff;
  --text: #1f2937;
  --primary: #2563eb;
  --primary-strong: #1d4ed8;
  --favorite: #e11d48;
  --success: #16a34a;
  --danger: #dc2626;
  --border: #dbe4ee;
  --radius: 12px;
}

body {
  background: var(--bg);
  color: var(--text);
  font-family: 'Segoe UI', sans-serif;
}

.app-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
}

.filters {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  margin: 16px 0;
}

.country-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 16px;
}

.button {
  background: var(--primary);
  color: #fff;
  border: 0;
  border-radius: 8px;
  padding: 10px 14px;
}

.button:hover {
  background: var(--primary-strong);
}

.favorite-button.is-active {
  background: var(--favorite);
}

.feedback-error {
  color: var(--danger);
}

.feedback-success {
  color: var(--success);
}
```

```css
@media (max-width: 768px) {
  .countries-grid {
    grid-template-columns: 1fr;
  }

  .filters {
    flex-direction: column;
  }
}
```

## Exercício Prático

1. Padronize visual de Home, Favoritos e Detalhes com as mesmas variáveis CSS.
2. Destaque visualmente país favoritado e link ativo da navegação.
3. Adapte filtros e grade para celular com media query.

## Erros Comuns

- Misturar estilos locais sem padrão global
- Não testar em telas menores
- Usar contraste fraco entre texto e fundo
- Estilizar só a Home e esquecer consistência nas rotas Favoritos e Detalhes

## Encerramento

Parabéns. Com esta etapa, a Lista de Países fica completa: dados reais, filtros, favoritos persistentes, rotas e interface final pronta para portfólio.
