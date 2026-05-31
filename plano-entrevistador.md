# Plano Entrevistador — Implementação do Agente Coach (Simulação de Entrevistas)

## Objetivo
Implementar o agente Coach para realizar simulações de entrevistas de emprego com o usuário, baseando-se nos dados do perfil, experiências, conhecimentos e projetos armazenados. O Coach deve avaliar o desempenho do usuário, fornecer feedback detalhado sobre erros cometidos, recomendações de melhoria e avaliar as chances de sucesso em uma entrevista real, utilizando linguagem formal adequada ao contexto profissional.

## Pré-requisitos
1. Firecrawl instalado e configurado com `FIRECRAWL_API_KEY` definida (para consultas de técnicas de entrevistas, se necessário)
2. Agentes Scout e Curator já implementados e funcionais (conforme `plano-aula-2.md` e `plano-aula-3.md`)
3. Arquivos de dados populados: `data/user-profile.md`, `data/job-search-results.md`, `data/course-search-results.md`
4. Ferramenta `spawn_agent` disponível no Maestro para disparar o Coach

## Contexto do Agente Coach

O Coach atua como um entrevistador profissional experiente, conduzindo uma simulação de entrevista realista com o usuário. Ele utiliza todos os dados disponíveis no sistema para formular perguntas pertinentes e avaliar as respostas.

**Fontes de Dados para a Entrevista**:
- `data/user-profile.md` — Experiências profissionais, habilidades, formação
- `data/job-search-results.md` — Vagas de interesse, requisitos das empresas
- `data/course-search-results.md` — Cursos realizados ou recomendados
- Conhecimento geral sobre técnicas de entrevistas (via Firecrawl, se necessário)

**Diferenciais do Coach**:
- Linguagem formal e profissional durante toda a interação
- Perguntas baseadas em evidências (projetos reais, experiências relatadas)
- Feedback construtivo e detalhado
- Avaliação realista das chances de aprovação

## Tarefas do Plano Entrevistador

### 1. Criar `skills/interview-simulation.md`
Procedimento completo de simulação de entrevistas contendo:
1. Leitura obrigatória de todos os arquivos de dados (`data/user-profile.md`, `data/job-search-results.md`, `data/course-search-results.md`)
2. Formulação de perguntas baseadas em:
   - Projetos mencionados no perfil
   - Habilidades técnicas e comportamentais
   - Experiências profissionais anteriores
   - Requisitos das vagas de interesse
3. Dinâmica da entrevista:
   - Apresentação formal do entrevistador
   - Perguntas abertas sobre experiências e projetos
   - Perguntas técnicas sobre habilidades declaradas
   - Perguntas comportamentais (soft skills)
   - Pergunta final de fechamento
4. Avaliação das respostas:
   - Clareza e objetividade
   - Uso adequado de terminologia técnica
   - Estruturação das respostas (ex: técnica STAR)
   - Postura profissional na comunicação
5. Formatação de feedback sem tabelas markdown (listas numeradas com pares chave-valor)
6. Tratamento de erros conforme diretrizes MoE

### 2. Criar `personas/coach.md`
Persona do Coach contendo:
1. Papel e responsabilidades: entrevistador simulado e avaliador
2. Estilo de comunicação: formal, profissional, estruturado
3. Ferramentas disponíveis (leitura de arquivos `data/`, Firecrawl via `terminal` para consultas pontuais)
4. Referência obrigatória à `skills/interview-simulation.md`
5. Entradas: todos os arquivos de dados do usuário
6. Formato de Response Envelope com seções específicas:
   - `estado`: sucesso ou erro
   - `resumo`: visão geral da entrevista
   - `dados`: transcrição da entrevista, avaliação, recomendações
   - `erros`: se houver falhas na leitura de dados
7. Critérios de avaliação:
   - Chance de aprovação (Alta, Média, Baixa)
   - Pontos fortes identificados
   - Pontos a melhorar
   - Erros específicos cometidos durante a entrevista
   - Recomendações de comunicação e postura

### 3. Atualizar `data/interview-results.md` (Novo arquivo de saída)
Esquema do arquivo de resultados do Coach:
```
Data da Entrevista: [AAAA-MM-DD HH:MM]

Configuração da Entrevista:
  Vagas de Referência: [baseado em data/job-search-results.md]
  Foco: [habilidades e experiências do usuário]

Transcrição da Entrevista:
  [Perguntas do Coach e respostas do usuário]

Avaliação Final:
  chance_aprovacao: [Alta/Média/Baixa]
  pontuacao_geral: [X de 10]
  
Pontos Fortes:
  1. [ponto forte 1]
  2. [ponto forte 2]

Pontos a Melhorar:
  1. [ponto a melhorar 1]
  2. [ponto a melhorar 2]

Erros Identificados na Comunicação:
  1. descricao: [erro cometido]
     recomendacao: [como corrigir]
     exemplo_correto: [forma adequada de falar]

Recomendações para Próximas Entrevistas:
  1. [recomendação 1]
  2. [recomendação 2]
```

### 4. Integrar Coach ao Maestro
1. Atualizar `personas/maestro.md` para disparar o Coach via `spawn_agent` quando o usuário selecionar a opção C (ou equivalente)
2. Opção C no menu do Maestro: "Simular entrevista de emprego"
3. Construir o Envelope de Despacho para o Coach com:
   - Todos os dados do usuário disponíveis
   - Contexto da vaga de interesse (se houver resultados do Scout)
4. Salvar resultados em `data/interview-results.md` e exibir ao usuário
5. O Maestro deve perguntar ao usuário se deseja fazer uma entrevista simulada após apresentar resultados do Scout ou Curator

### 5. Protocolo de Handoff do Coach

**Envelope de Despacho** (construído pelo Maestro):

```
## DESPACHO: COACH
### referencia_persona
[Conteúdo completo de personas/coach.md]

### tarefa
Realizar simulação de entrevista de emprego com o usuário, avaliar desempenho e fornecer feedback detalhado

### perfil_usuario
[Conteúdo de data/user-profile.md]

### contexto
Vagas de Interesse:
  [extraído de data/job-search-results.md]

Cursos Realizados/Recomendados:
  [extraído de data/course-search-results.md]

Experiências e Projetos:
  [extraído de data/user-profile.md]

### saida_esperada
Envelope de resposta com estado, resumo, dados (transcrição, avaliação, recomendações) e erros se houver
```

**Formato de dados da resposta:**
```
Transcrição da Entrevista:
  [Perguntas e respostas]

Avaliação:
  chance_aprovacao: [Alta/Média/Baixa]
  pontuacao: [X de 10]
  
Erros Identificados:
  1. descricao: [erro]
     recomendacao: [correção]
     
Recomendações:
  1. [recomendação]
```

### 6. Testes de Validação
1. Cenário: Dados completos → Coach realiza entrevista fluida, faz perguntas pertinentes, fornece feedback detalhado
2. Cenário: Dados parciais → Coach trabalha com o que está disponível, avisa sobre lacunas
3. Cenário: Usuário comete erros de comunicação → Coach identifica, explica e dá exemplos corretos
4. Cenário: Leitura de arquivos falha → Coach reporta erro no campo `erros` e não prossegue
5. Cenário: Usuário responde bem → Coach avalia chance alta e reforça pontos positivos

## Cronograma Sugerido (90 minutos)
1. 00-10 min: Revisão dos planos anteriores e estrutura existente
2. 10-35 min: Criação de `skills/interview-simulation.md`
3. 35-55 min: Criação de `personas/coach.md`
4. 55-70 min: Integração do Coach com o Maestro (atualização do menu e handoff)
5. 70-85 min: Testes de simulação e avaliação
6. 85-90 min: Ajustes finais e validação end-to-end

## Regras de Implementação
1. Seguir diretrizes MoE: sem tabelas markdown, caminhos relativos a `data/`, não inventar dados
2. O Coach deve personificar um entrevistador real: linguagem formal, perguntas estruturadas, avaliação imparcial
3. O Coach NÃO deve escrever código, apenas personificar o entrevistador
4. Todas as perguntas devem ser baseadas em dados reais do usuário (projetos, experiências)
5. O feedback deve ser construtivo, específico e incluir exemplos de como falar corretamente
6. Se faltarem dados para uma pergunta específica, o Coach deve adaptar e informar ao usuário
7. Priorizar Firecrawl apenas para consultas pontuais sobre técnicas de entrevistas, se necessário
8. O Coach deve explicar detalhadamente os erros cometidos e como melhorar a comunicação

## Fluxo

```
Usuário seleciona C no menu (Simular entrevista)
        │
        ▼
Maestro verifica se há dados mínimos (user-profile.md)
        │
        ▼
Maestro coleta dados de todos os arquivos disponíveis
        │
        ▼
Maestro constrói envelope de despacho para Coach
        │
        ▼
spawn_agent dispara Coach com persona + contexto completo
        │
        ▼
Coach lê todos os dados e inicia entrevista formal
        │
        ▼
Coach faz perguntas baseadas em projetos e experiências
        │
        ▼
Usuário responde às perguntas (interação conversacional)
        │
        ▼
Coach avalia respostas em tempo real
        │
        ▼
Coach fornece feedback final com:
  - Avaliação de chances (Alta/Média/Baixa)
  - Erros identificados e como corrigir
  - Recomendações de comunicação
  - Exemplos de respostas adequadas
        │
        ▼
Maestro salva em data/interview-results.md → exibe ao usuário → retorna ao menu
```

## Entregável
Agente Coach totalmente funcional, simulação de entrevistas end-to-end integrada ao Maestro, com avaliação detalhada, feedback construtivo e recomendações específicas de comunicação profissional, utilizando linguagem formal adequada ao contexto de entrevistas corporativas.
