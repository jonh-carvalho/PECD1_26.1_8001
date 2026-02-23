---
id: avaliacao_suplementar
title: Avaliação Suplementar – Plataforma de Mobilidade Universitária
---

## 1. Tema e Objetivos

Desenvolver uma **Plataforma de Mobilidade Universitária** onde estudantes organizam caronas solidárias, trajetos de bicicletas compartilhadas e shuttle internos do campus. O sistema conecta motoristas voluntários, passageiros e o comitê de mobilidade do centro acadêmico.

Objetivos pedagógicos:

1. Consolidar o uso de Vite + React para criar SPA responsiva com múltiplas rotas.
2. Integrar um backend leve via JSON Server, projetando modelos de dados coerentes com mobilidade urbana.
3. Demonstrar capacidade de traduzir descobertas de pesquisa e documentação enxuta em funcionalidades reais.
4. Exercitar métricas de impacto (uso das rotas, taxa de ocupação, feedback de segurança).

Este tema não herda requisitos de `cenario.md`, permitindo liberdade para explorar novos fluxos, personas e dados.

---

## 2. Requisitos do Aplicativo

### Papéis

- **Comitê de Mobilidade (Admin):** valida motoristas, cria rotas oficiais, monitora ocupação dos shuttles.
- **Motorista Estudante:** cadastra veículo, define rotas/horários, aceita passageiros e registra ocorrências.
- **Passageiro Estudante:** busca rotas, solicita vaga, avalia viagens e registra incidentes.
- **Equipe de Segurança/Infra:** acompanha indicadores de risco e bloqueia rotas/motoristas quando necessário.
- **Visitante:** visualiza painel de rotas públicas e métricas agregadas (modo leitura).

### Funcionalidades essenciais

1. **Catálogo de Rotas e Viagens:** listagem filtrável (tipo: carona, bike, shuttle; horário; lotação) com CTA adaptado ao papel logado (reservar, editar, aprovar).
2. **Ciclo completo de Carona:** motoristas criam viagens, definem vagas disponíveis e status (agendada, em andamento, concluída). Passageiros solicitam vaga, recebem confirmação e status acompanha o fluxo.
3. **Mapa de Calor (simulado):** painel resume métricas (rotas ativas, ocupação média, alertas) e destaca rotas críticas; pode usar gráficos/bar charts simples ou cards com ícones.
4. **Registro de Ocorrências/Feedback:** alunos relatam problemas (ex.: atraso, segurança) e o comitê resolve/fecha tickets.
5. **Histórico de Utilização:** cada usuário visualiza suas viagens passadas, notas dadas/recebidas e badges (opcional).

### Funcionalidades opcionais

- Integração com mapa real (Leaflet ou Mapbox) usando coordenadas fictícias.
- Sistema de gamificação (pontos por viagens sustentáveis, ranking).
- Modo “planejamento de evento” onde a plataforma cria rotas temporárias.
- Notificações locais (toasts, e-mails simulados) para confirmações ou cancelamentos.
- Exportação CSV de ocorrências para auditoria.

---

## 3. Documentação e Entregas Simplificadas

Para equilibrar documentação e desenvolvimento, use apenas três artefatos-chave na pasta `docs/Design Thinking/`:

1. **`pesquisa.md` (adaptado):**  
   - Resuma personas, dores e métricas macro de mobilidade em no máximo duas páginas.  
   - Foque em dados que realmente alimentem decisões de produto (ex.: “60% dos estudantes moram fora do raio do shuttle”).  
   - Use tabelas ou bullets; imagens são opcionais.

2. **`5w2h.md` (adaptado ao tema):**  
   - Atualize What/Who/Why/How com informações da plataforma de mobilidade.  
   - Inclua, no “How much”, estimativas de esforço/tarefas do app Vite React.

3. **`as.md` (utilize o conteúdo do as em um arquivo as.md no seu repositório):**  
   - Serve como a especificação viva do produto (requisitos, dados, critérios de avaliação).  
   - Referencie nele os outros documentos quando necessário.

> Demais documentos (brainstorm, protótipos) tornam-se opcionais. Se utilizados, cite-os no README apenas como material extra. O foco é alinhar discovery mínimo e entrega técnica.

**Fluxo sugerido de documentação x desenvolvimento:**

1. Atualizar `pesquisa.md` e `5w2h.md` com insights da mobilidade.
2. Definir o backlog principal diretamente neste arquivo.
3. Construir o app Vite com base nessas decisões, mantendo o README como espinha dorsal do desenvolvimento.

---

## 4. Stack Técnica, Dados e Avaliação

### Stack mínima

- **Vite + React** (JS ou TS) com módulos separados em `pages/`, `components/`, `services/`, `hooks/`.
- **React Router** com rotas: `/rotas`, `/minhas-viagens`, `/painel`, `/ocorrencias`.
- **Gerenciamento de estado:** Context API ou Zustand para usuário atual, filtros e notificações.
- **Fetch** com instância configurada (`src/services/api.js`) apontando para `http://localhost:3333`.


### Estrutura recomendada no JSON Server (`db.json`)

```json
{
  "rotas": [
    {
      "id": 1,
      "tipo": "carona",
      "origem": "Campus Centro",
      "destino": "Residencial Norte",
      "horario": "07:30",
      "vagas": 3,
      "ocupacao": 1,
      "status": "aberta",
      "motoristaId": 10
    }
  ],
  "usuarios": [
    {
      "id": 10,
      "nome": "Ana Ribeiro",
      "papel": "motorista",
      "veiculo": "Carro 1.0 - PVX1234",
      "reputacao": 4.8
    }
  ],
  "reservas": [
    {
      "id": 200,
      "rotaId": 1,
      "passageiro": "Carlos Mendes",
      "status": "confirmada"
    }
  ],
  "ocorrencias": [
    {
      "id": 400,
      "rotaId": 1,
      "autor": "Carlos Mendes",
      "categoria": "atraso",
      "descricao": "Saída ocorreu 20 minutos depois.",
      "status": "aberta"
    }
  ],
  "metricas": [
    {
      "id": 1,
      "rotasAtivas": 12,
      "ocupacaoMedia": 0.78,
      "alertas": 2
    }
  ]
}
```

### Scripts recomendados

- `npm install`  
- `npm run dev` (porta padrão 5173)  
- `npx json-server --watch db.json --port 3333`  
- `npm run test` (Vitest/Jest)

Incluir todos no README com instruções rápidas de setup.

### Critérios de avaliação

| Critério | Descrição | Peso |
| --- | --- | --- |
| Funcionalidade | Implementou catálogo, fluxo de carona, painel de métricas e ocorrências | 35% |
| Qualidade técnica | Estrutura do código, consumo da API, estado global, testes | 25% |
| UX & Acessibilidade | Navegação clara, responsividade, feedback visual e mensagens de erro | 20% |
| Documentação integrada | Atualização de `pesquisa.md`, `5w2h.md`, README e clareza deste arquivo | 10% |
| Inovação/opcionais | Uso de funcionalidades extras (mapa, gamificação, exportação) | 10% |

### Boas práticas sugeridas

- Tratar loading/error states em todas as telas (skeletons, toasts).
- Centralizar schemas/validações para reaproveitamento.
- Padronizar componentes (Buttons, Cards, StatusTag) para acelerar o desenvolvimento.
- Criar seed inicial no `db.json` alinhado às personas definidas em `pesquisa.md`.

---

