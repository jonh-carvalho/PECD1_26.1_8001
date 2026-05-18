---
id: 15_Estilizacao_Avancada
title: 15 - Estilização Avançada
---

# 15 - Estilização Avançada

Este módulo encerra a trilha com foco em acabamento visual, consistência e responsividade. A lógica principal da aplicação já está pronta; agora a meta é melhorar experiência e apresentação.

## Objetivos do Módulo

- Organizar estilo com padrão reutilizável
- Definir paleta, espaçamento e tipografia consistentes
- Melhorar responsividade para diferentes telas
- Aplicar estados visuais para loading, erro e interação

## Pré-requisitos

- Aplicação com rotas e listagem funcionando
- Componentes separados por responsabilidade
- CSS básico ou CSS Modules

## Conceito Principal

Estilização avançada não é só aparência. Ela melhora legibilidade, navegação e previsibilidade de uso.

## Exemplo Guiado

```css
:root {
  --bg: #f7f9fc;
  --surface: #ffffff;
  --text: #1f2937;
  --primary: #2563eb;
  --border: #dbe4ee;
  --radius: 12px;
}

body {
  background: var(--bg);
  color: var(--text);
  font-family: 'Segoe UI', sans-serif;
}

.card {
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

1. Aplique variáveis CSS para cores e espaçamentos.
2. Padronize botões e cards em toda a aplicação.
3. Adapte a tela para celular com media query.

## Erros Comuns

- Misturar estilos locais sem padrão global
- Não testar em telas menores
- Usar contraste fraco entre texto e fundo

## Encerramento

Parabéns. Com esta etapa, a Lista de Países fica completa: dados reais, filtros, favoritos persistentes, rotas e interface final pronta para portfólio.
