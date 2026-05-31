# Plano Aula 2 — Implementação do Agente Scout (Buscador de Vagas)

## Objetivo
Implementar o agente Scout para busca de vagas de emprego, utilizando o Firecrawl CLI como método primário, com fallback para a ferramenta de acesso web nativa do Zed, e validar o fluxo end-to-end com o Maestro.

## Pré-requisitos
1. Firecrawl instalado e configurado com `FIRECRAWL_API_KEY` definida
2. Ferramenta de acesso web nativa (`fetch`) disponível no Zed
3. Estrutura de diretórios do projeto conforme `plano.md` já criada

## Tarefas da Aula 2
### 1. Criar `skills/job-search.md`
Procedimento completo de busca de vagas contendo:
1. Uso prioritário do Firecrawl CLI via `terminal` para busca e extração de vagas
2. Fallback para a ferramenta `fetch` nativa do Zed caso o Firecrawl falhe
3. Sites alvo obrigatórios: InfoJobs, Vagas, Indeed
4. Extração de dados, correspondência de habilidades, formatação de resposta sem tabelas markdown
5. Tratamento de erros conforme diretrizes MoE

### 2. Criar `personas/scout.md`
Persona do Scout contendo:
1. Papel e responsabilidades
2. Ferramentas disponíveis (Firecrawl CLI, `fetch` nativo)
3. Referência obrigatória à `skills/job-search.md` e `skills/firecrawl.md`
4. Formato de Response Envelope
5. Regras de tratamento de erros

### 3. Integrar Scout ao Maestro
1. Atualizar `personas/maestro.md` para disparar o Scout via `spawn_agent` quando o usuário selecionar a opção A
2. Construir o Envelope de Despacho conforme `plano.md`
3. Salvar resultados em `data/job-search-results.md` e exibir ao usuário

### 4. Testes de Validação
1. Cenário: Firecrawl funcional → Retorno de vagas com correspondência de habilidades
2. Cenário: Firecrawl falha → Fallback para `fetch` nativo em InfoJobs, Vagas, Indeed
3. Cenário: Nenhum resultado → Alerta ao usuário e sugestão de ampliar termos

## Cronograma Sugerido (90 minutos)
1. 00-15 min: Revisão do `plano.md` e estrutura existente
2. 15-45 min: Criação de `skills/job-search.md` e `personas/scout.md`
3. 45-60 min: Integração do Scout com o Maestro
4. 60-75 min: Testes de fluxo e fallback
5. 75-90 min: Ajustes finais e validação end-to-end

## Regras de Implementação
1. Seguir diretrizes MoE: sem tabelas markdown, caminhos relativos a `data/`, não inventar dados
2. Priorizar Firecrawl CLI; usar `fetch` nativo apenas como fallback documentado
3. Sites de busca obrigatórios: InfoJobs, Vagas, Indeed
4. O agente NÃO deve escrever código, apenas personificar o Scout
