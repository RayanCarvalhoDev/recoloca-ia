# Plano Aula 3 — Implementação do Agente Curator (Buscador de Cursos)

## Objetivo
Implementar o agente Curator para busca de cursos que complementem as habilidades faltantes do usuário em relação às vagas encontradas, utilizando o Firecrawl CLI como método primário, com foco em cursos gratuitos, e validar o fluxo end-to-end com o Maestro.

## Pré-requisitos
1. Firecrawl instalado e configurado com `FIRECRAWL_API_KEY` definida
2. Agente Scout já implementado e funcional (conforme `plano-scout.md`)
3. Arquivo `data/job-search-results.md` sendo gerado pelo Scout com as habilidades faltantes
4. Arquivos `data/user-profile.md` e `data/personality-quiz.md` atualizados

## Contexto do Agente Curator

O Curator analisa as habilidades faltantes identificadas pelo Scout em `data/job-search-results.md` e busca cursos que as complementem. O foco é em cursos gratuitos, mas também pode sugerir opções pagas quando relevantes.

**Fontes de Dados para Cursos**:
- Sites de cursos gratuitos: Coursera, edX, Udemy (cursos gratuitos), Fundação Bradesco, SENAI, SEBRAE
- Plataformas de cursos em geral: Alura, Udemy, Coursera, edX
- Documentação oficial e tutoriais gratuitos encontrados via Firecrawl

## Tarefas da Aula 3

### 1. Criar `skills/course-search.md`
Procedimento completo de busca de cursos contendo:
1. Uso prioritário do Firecrawl CLI via `terminal` para busca de cursos
2. Extração de habilidades faltantes de `data/job-search-results.md`
3. Comandos de busca por habilidade específica: `firecrawl search "curso gratuito [habilidade] online" --json`
4. Filtragem por cursos gratuitos vs pagos
5. Extração de dados do curso: título, plataforma, carga horária, nível, link, gratuito/pago
6. Correspondência entre curso e habilidade faltante
7. Formatação de resposta sem tabelas markdown (listas numeradas com pares chave-valor)
8. Tratamento de erros conforme diretrizes MoE
9. Fallback: se busca por habilidade específica falhar, tentar termos alternativos; se todos falharem, pular essa habilidade e continuar com as restantes

### 2. Criar `personas/curator.md`
Persona do Curator contendo:
1. Papel e responsabilidades: buscar cursos que complementem habilidades faltantes
2. Ferramentas disponíveis (Firecrawl CLI via `terminal`)
3. Referência obrigatória à `skills/course-search.md` e `skills/firecrawl.md`
4. Entradas: habilidades faltantes de `data/job-search-results.md`, perfil do usuário de `data/user-profile.md`
5. Formato de Response Envelope
6. Regras de tratamento de erros
7. Prioridade: cursos gratuitos primeiro, depois pagos se não houver opções gratuitas

### 3. Atualizar `data/course-search-results.md` (Novo arquivo de saída)
Esquema do arquivo de resultados do Curator:
```
Data da Busca: [AAAA-MM-DD HH:MM]
Habilidades Faltantes Identificadas:
  - [habilidade1]
  - [habilidade2]

Cursos Recomendados:
1. titulo: [título do curso]
   plataforma: [nome da plataforma]
   habilidade_relacionada: [habilidade que o curso complementa]
   carga_horaria: [duração ou carga horária]
   nivel: [Iniciante, Intermediário, Avançado]
   gratuito: [Sim/Não]
   link: [URL]
   descricao_curta: [breve descrição]

2. [próximo curso no mesmo formato]
```

### 4. Integrar Curator ao Maestro
1. Atualizar `personas/maestro.md` para disparar o Curator via `spawn_agent` quando apropriado
2. Opção B no menu do Maestro: "Buscar cursos para complementar habilidades"
3. Construir o Envelope de Despacho para o Curator com:
   - Habilidades faltantes de `data/job-search-results.md`
   - Perfil do usuário de `data/user-profile.md`
4. Salvar resultados em `data/course-search-results.md` e exibir ao usuário

### 5. Protocolo de Handoff do Curator

**Envelope de Despacho** (construído pelo Maestro):

```
## DESPACHO: CURATOR
### referencia_persona
[Conteúdo completo de personas/curator.md]

### tarefa
Buscar cursos gratuitos que complementem as habilidades faltantes identificadas nas vagas

### perfil_usuario
[Conteúdo de data/user-profile.md]

### contexto
Habilidades Faltantes:
  [lista de habilidades faltantes extraídas de data/job-search-results.md]

Preferência: Cursos gratuitos
Plataformas alvo: Coursera, edX, Udemy (gratuitos), Fundação Bradesco, SENAI, SEBRAE

### saida_esperada
Envelope de resposta com estado, resumo, dados (lista de cursos) e erros se houver
```

**Formato de dados da resposta:**
```
1. titulo: [título do curso]
   plataforma: [nome da plataforma]
   habilidade_relacionada: [habilidade]
   carga_horaria: [duração]
   nivel: [nível]
   gratuito: [Sim/Não]
   link: [URL]

2. [próximo curso no mesmo formato]
```

### 6. Testes de Validação
1. Cenário: Scout executado previamente → Curator identifica habilidades faltantes e busca cursos gratuitos
2. Cenário: Firecrawl funcional → Retorno de cursos com correspondência de habilidades
3. Cenário: Firecrawl falha para uma habilidade → Fallback para termos alternativos ou pular habilidade
4. Cenário: Nenhum curso gratuito encontrado → Sugerir cursos pagos com aviso
5. Cenário: `data/job-search-results.md` vazio → Alerta ao usuário para executar o Scout primeiro

## Cronograma Sugerido (90 minutos)
1. 00-10 min: Revisão do `plano.md`, `plano-scout.md` e estrutura existente
2. 10-35 min: Criação de `skills/course-search.md`
3. 35-55 min: Criação de `personas/curator.md`
4. 55-70 min: Integração do Curator com o Maestro (atualização do menu e handoff)
5. 70-85 min: Testes de fluxo e fallback
6. 85-90 min: Ajustes finais e validação end-to-end

## Regras de Implementação
1. Seguir diretrizes MoE: sem tabelas markdown, caminhos relativos a `data/`, não inventar dados
2. Priorizar Firecrawl CLI; se falhar consistentemente, reportar erro no campo `erros`
3. Foco em cursos gratuitos: sempre verificar se o curso é gratuito antes de recomendar
4. O agente NÃO deve escrever código, apenas personificar o Curator
5. Se uma busca falhar para uma habilidade específica, tentar fallback; se o fallback também falhar, pular e continuar com as restantes
6. Todos os caminhos de arquivo devem ser relativos à raiz do projeto com prefixo `data/`
7. O Curator deve ler `data/job-search-results.md` para identificar as habilidades faltantes antes de iniciar a busca

## Fluxo

```
Usuário seleciona B no menu (ou opção equivalente)
        │
        ▼
Maestro verifica se data/job-search-results.md existe e tem dados
        │
        ▼
Maestro extrai habilidades faltantes dos resultados do Scout
        │
        ▼
Maestro constrói envelope de despacho para Curator
        │
        ▼
spawn_agent dispara Curator com persona + contexto
        │
        ▼
Curator executa firecrawl search para cada habilidade faltante
        │
        ▼
Curator filtra cursos gratuitos e faz correspondência com habilidades
        │
        ▼
Curator retorna envelope de resposta com cursos recomendados
        │
        ▼
Maestro salva em data/course-search-results.md → exibe ao usuário → retorna ao menu
```

## Entregável
Agente Curator totalmente funcional, busca de cursos end-to-end integrada ao Maestro, com foco em cursos gratuitos que complementem as habilidades faltantes identificadas pelo Scout.
