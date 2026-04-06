# Brainstorm - Sistema de Atividades Acadêmicas Complementares (AAC)

## 1) Objetivo Central
Criar um sistema web que permita registrar, validar e acompanhar AAC de forma transparente para alunos, coordenação e orientadores, reduzindo atrasos e inconsistências na colação de grau.

## 2) Problema em Frases Curtas
- Aluno não sabe quanto falta para concluir AAC.
- Coordenação perde tempo com validação manual e retrabalho.
- Documentos chegam incompletos ou fora do padrão.
- Regras mudam por curso/categoria e geram confusão.
- Risco de bloqueio na colação de grau por falta de horas.

## 3) Perfis de Usuário
- Aluno: submete atividade, acompanha status, recebe alertas de pendência.
- Avaliador/Orientador: valida evidências e carga horária.
- Coordenação: configura regras, monitora indicadores e emite relatórios.
- Secretaria (opcional): apoio operacional e auditoria documental.

## 4) Perguntas Norteadoras (Para oficina de ideação)
- Como tornar o envio de comprovantes quase "à prova de erro"?
- Como evitar que o aluno concentre horas em uma única categoria?
- Como dar feedback rápido sobre indeferimento e correções?
- Como separar claramente atividades internas e externas?
- Como prever cedo quem está em risco de não cumprir horas?

## 5) Regras de Negócio (Contexto regulatório)
- Três eixos acadêmicos: Ensino, Pesquisa e Extensão.
- Estágio obrigatório não conta como AAC.
- Estágio não obrigatório pode contar, respeitando limite institucional.
- Atividades internas e externas seguem fluxos distintos de validação.
- Atividades externas exigem comprovante com entidade emissora, carga horária, data e verificação de autenticidade.
- Certificados emitidos por pessoa física devem ser rejeitados automaticamente.
- Atividades anteriores ao ingresso no curso não devem ser aceitas.
- Limites por categoria (exemplos):
	- Estágio não obrigatório: até 100h
	- Monitoria: até 100h
	- Idiomas: até 30h
	- Defesas de TCC: até 10h

## 6) Ideias de Funcionalidades (MVP)
- Dashboard do aluno com:
	- horas totais concluídas x exigidas
	- progresso por eixo (Ensino/Pesquisa/Extensão)
	- progresso por categoria e limite
	- prazo semestral e alertas de risco
- Formulário inteligente de submissão:
	- checklist obrigatório por tipo de atividade
	- validação automática de campos obrigatórios
	- upload de PDF/imagem com pré-visualização
- Fila de validação para avaliadores:
	- aprovar, indeferir ou solicitar ajuste
	- comentários padronizados por motivo
	- SLA de resposta (ex.: 7 dias)
- Histórico e rastreabilidade:
	- linha do tempo de cada submissão
	- motivo de decisão registrado
- Relatórios da coordenação:
	- alunos em risco
	- taxa de aprovação/indeferimento
	- tempo médio de validação

## 7) Ideias Diferenciadoras (Fase 2)
- OCR para ler certificado e sugerir campos automaticamente.
- Verificação de autenticidade por QR Code/link.
- Motor de regras configurável por curso/período.
- Recomendador de atividades (MEC, FGV, Bradesco, Sebrae etc.) com base em horas faltantes.
- Simulador "E se": aluno testa cenários para completar horas mais rápido.
- Painel preditivo de risco de não colação de grau.

## 8) Fluxo Ideal (Aluno)
1. Escolhe tipo de atividade (interna/externa).
2. Sistema mostra requisitos e limite aplicável.
3. Aluno anexa comprovante e preenche dados.
4. Validação automática inicial (campos/regras).
5. Entra na fila de análise humana.
6. Resultado: aprovado, indeferido ou ajuste solicitado.
7. Horas atualizadas no painel e no extrato de AAC.

## 9) Fluxo Ideal (Coordenação)
1. Define regras por curso e semestre.
2. Monitora fila de análise e SLA.
3. Acompanha indicadores de risco por turma.
4. Emite relatório de conformidade para fechamento semestral.

## 10) Indicadores de Sucesso
- Redução do tempo médio de validação.
- Redução de indeferimentos por erro documental.
- Percentual de alunos com >= 80% das horas no penúltimo período.
- Queda no número de bloqueios de colação por AAC pendente.
- Satisfação dos usuários (aluno e avaliador).

## 11) Riscos e Mitigações
- Risco: regras diferentes por curso gerarem inconsistência.
	- Mitigação: motor de regras versionado por curso.
- Risco: fraude documental.
	- Mitigação: verificação por QR/link + auditoria amostral.
- Risco: gargalo na validação manual.
	- Mitigação: priorização por prazo e triagem automática.
- Risco: baixa adesão dos alunos no início do semestre.
	- Mitigação: notificações e trilha guiada de primeiro envio.

## 12) Brainstorm de Telas
- Tela 1: Visão Geral do Aluno (progresso, alertas, próximos passos).
- Tela 2: Nova Submissão (formulário adaptativo por tipo).
- Tela 3: Minhas Solicitações (status + histórico + pendências).
- Tela 4: Fila do Avaliador (filtros por curso, prazo, tipo).
- Tela 5: Configuração de Regras (coordenação).
- Tela 6: Relatórios e Exportações (CSV/PDF).

## 13) Ideias para Sprint 1
- Cadastro de usuários e autenticação.
- Cadastro e submissão de atividades com anexos.
- Regras básicas de limite por categoria.
- Aprovação/indeferimento com justificativa.
- Dashboard inicial com progresso de horas.

## 14) Frases-Guia do Produto
- "Nenhum aluno descobre no fim do curso que faltam horas."
- "Toda decisão de validação é clara, rastreável e auditável."
- "Cumprir AAC deve ser simples para o aluno e escalável para a instituição."
