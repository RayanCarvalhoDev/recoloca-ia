# Course Search Skill — Busca de Cursos

## Visão Geral
Skill obrigatória para o agente Curator. Define o procedimento completo de busca de cursos que complementem as habilidades faltantes do usuário, priorizando opções gratuitas.

## Ferramentas Utilizadas
- `terminal`: Executar comandos do Firecrawl CLI (`firecrawl search`, `firecrawl scrape`)
- `read_file`: Ler `data/job-search-results.md` e `data/user-profile.md`

## Pré-requisitos
1. Arquivo `data/job-search-results.md` deve existir e conter as habilidades faltantes das vagas
2. Arquivo `data/user-profile.md` deve estar atualizado com as habilidades atuais do usuário
3. Firecrawl configurado com `FIRECRAWL_API_KEY` definida

## Procedimento Passo a Passo

### 1. Extrair Habilidades Faltantes
1. Ler `data/job-search-results.md` usando `read_file`
2. Identificar todas as habilidades listadas em `habilidades_faltantes` para cada vaga
3. Consolidar uma lista única de habilidades faltantes (remover duplicatas)
4. Se não houver habilidades faltantes, retornar mensagem informando que não há gaps a serem preenchidos

### 2. Buscar Cursos por Habilidade
Para cada habilidade faltante na lista consolidada:
1. Executar comando de busca via `terminal`:
   ```
   firecrawl search "curso gratuito [habilidade] online" --json
   ```
2. Se a busca não retornar resultados, tentar fallback com termos alternativos:
   ```
   firecrawl search "curso [habilidade] gratuito" --json
   firecrawl search "[habilidade] course free online" --json
   ```
3. Se todos os fallbacks falharem para uma habilidade específica, pular essa habilidade e continuar com as restantes, registrando o erro no campo `erros` da resposta

### 3. Filtrar Cursos Gratuitos
Para cada resultado da busca:
1. Verificar se o curso é gratuito através do título, descrição ou URL
2. Priorizar resultados de plataformas conhecidas por cursos gratuitos:
   - Coursera (cursos auditáveis gratuitamente)
   - edX (cursos gratuitos com opção de certificado pago)
   - Udemy (cursos gratuitos marcados como "Free")
   - Fundação Bradesco (100% gratuitos)
   - SENAI (cursos gratuitos)
   - SEBRAE (cursos gratuitos)
3. Para cada curso promissor, executar `firecrawl scrape <url> --format markdown` para obter detalhes completos
4. Se o scrape falhar, usar título e descrição do resultado da busca

### 4. Extrair Dados do Curso
Para cada curso válido, extrair:
1. titulo: Título completo do curso
2. plataforma: Nome da plataforma onde o curso está hospedado
3. habilidade_relacionada: Habilidade faltante que o curso complementa
4. carga_horaria: Duração ou carga horária informada
5. nivel: Iniciante, Intermediário ou Avançado
6. gratuito: Sim (se totalmente gratuito) ou Não (se houver custo)
7. link: URL direta para o curso
8. descricao_curta: Breve descrição do conteúdo programático

### 5. Corresponder Curso com Habilidade
1. Verificar se o conteúdo do curso realmente aborda a habilidade faltante
2. Usar correspondência de strings sem distinção de maiúsculas/minúsculas
3. Se o curso não for relevante para a habilidade, descartar

### 6. Formatar Resposta
Usar listas numeradas com pares chave-valor (sem tabelas markdown):
```
1. titulo: [título do curso]
   plataforma: [nome da plataforma]
   habilidade_relacionada: [habilidade]
   carga_horaria: [duração]
   nivel: [nível]
   gratuito: [Sim/Não]
   link: [URL]
   descricao_curta: [breve descrição]

2. [próximo curso no mesmo formato]
```

### 7. Tratamento de Erros
1. Se `firecrawl search` falhar consistentemente para todas as habilidades, reportar erro no campo `erros` da resposta
2. Se `firecrawl scrape` falhar em uma URL específica, usar dados da busca e anotar falha
3. Se não houver cursos gratuitos encontrados, informar e sugerir cursos pagos com aviso claro
4. Nunca inventar dados de cursos. Se a busca falhar, reportar o erro exato

## Regras MoE
- Sem tabelas markdown em nenhuma saída
- Caminhos de arquivo relativos à raiz do projeto com prefixo `data/`
- Não escrever código, apenas seguir o procedimento como persona
- Se uma ferramenta falhar, relatar no campo `erros` e não continuar silenciosamente
