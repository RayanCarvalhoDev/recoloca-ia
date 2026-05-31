# Skill: Busca de Vagas (job-search)

## Objetivo
Realizar busca de vagas de emprego usando o Firecrawl CLI como método primário, com fallback para a ferramenta de acesso web nativa (`fetch`) do Zed, extrair dados das vagas, corresponder habilidades do usuário e formatar resultados.

## Ferramentas Utilizadas
1. Firecrawl CLI (prioritário): Executar comandos via `terminal` do Zed
   - `firecrawl search`: Busca de vagas agregando múltiplos sites
   - `firecrawl scrape`: Extração de detalhes completos de uma vaga específica
2. Ferramenta de acesso web nativa (fallback): `fetch` do Zed, usada apenas se o Firecrawl falhar consistentemente
3. Leitura de arquivos: `read_file` para carregar `data/user-profile.md`

## Sites Alvo
Buscar vagas prioritariamente nos seguintes sites:
1. InfoJobs (https://www.infojobs.com.br)
2. Vagas (https://www.vagas.com.br)
3. Indeed (https://www.indeed.com.br)

## Procedimento Passo a Passo
### Passo 1: Carregar Perfil do Usuário
1. Ler o arquivo `data/user-profile.md` usando `read_file`
2. Extrair os seguintes campos obrigatórios:
   1. area_de_interesse: [valor]
   2. localizacao: [valor]
   3. nivel_de_experiencia: [Júnior/Pleno/Sênior]
   4. habilidades_atuais: [lista separada por vírgulas]

### Passo 2: Busca Primária com Firecrawl CLI
1. Executar o seguinte comando via `terminal` (diretório raiz do projeto):
   ```bash
   firecrawl search "vagas [area_de_interesse] [localizacao] site:infojobs.com.br OR site:vagas.com.br OR site:indeed.com.br" --json
   ```
2. O comando retorna JSON com: url, titulo, descricao, cargo para cada resultado
3. Se o comando falhar ou não retornar resultados, prosseguir para o Passo 3 (Fallback)

### Passo 3: Fallback para Ferramenta de Acesso Web Nativa (fetch)
Se o Firecrawl falhar, usar a ferramenta `fetch` do Zed para buscar vagas diretamente nos sites alvo:
1. InfoJobs: `fetch https://www.infojobs.com.br/vagas-de-emprego/[area]-[localizacao].aspx`
2. Vagas: `fetch https://www.vagas.com.br/vagas/[area]?cidade=[localizacao]`
3. Indeed: `fetch https://www.indeed.com.br/vagas?q=[area]&l=[localizacao]`
4. Extrair títulos, empresas e links das páginas retornadas (formato HTML bruto)

### Passo 4: Extração de Detalhes das Vagas
Para cada uma das até 5 vagas encontradas:
1. Tentar `firecrawl scrape <url> --format markdown` via `terminal`
2. Se falhar, usar o título e descrição obtidos no resultado da busca inicial
3. Se estiver usando fallback do `fetch`, extrair o que estiver disponível no HTML

### Passo 5: Extração de Requisitos e Habilidades
Das descrições das vagas (markdown ou HTML), extrair:
1. Nome da empresa (da URL, título ou descrição)
2. Localização (cidade ou "Remoto")
3. Habilidades requeridas (linguagens, frameworks, ferramentas mencionadas)

### Passo 6: Correspondência de Habilidades
1. Comparar habilidades requeridas com as habilidades atuais do usuário
2. Usar correspondência case-insensitive (ignorar maiúsculas/minúsculas)
3. Contar quantas habilidades correspondem vs. quantas faltam
4. Listar habilidades_correspondentes e habilidades_faltantes

### Passo 7: Filtragem por Nível de Experiência
1. Verificar se a vaga menciona nível (Júnior, Pleno, Sênior, etc.)
2. Priorizar vagas que correspondam ao nível do usuário
3. Se não houver vagas do nível exato nos primeiros resultados, incluir vagas de nível adjacente anotando a discrepância

### Passo 8: Formatação da Resposta
Retornar até 5 vagas no seguinte formato (sem tabelas markdown):

```
1. titulo: [título da vaga]
   empresa: [nome da empresa]
   localizacao: [cidade ou Remoto]
   link: [URL]
   habilidades_correspondentes: [habilidade1, habilidade2]
   habilidades_faltantes: [habilidade3, habilidade4]
   contagem_correspondencia: [X de Y habilidades correspondem]

2. [próxima vaga no mesmo formato]
```

## Tratamento de Erros
1. **Falha no firecrawl search**: Reportar erro exato, tentar fallback com `fetch`
2. **Falha no firecrawl scrape**: Usar dados da busca inicial, anotar falha na vaga
3. **Falha no fetch**: Reportar erro, não continuar silenciosamente
4. **Sem resultados**: Informar ao usuário e sugerir ampliar os termos de busca
5. **Nunca inventar dados**: Se uma extração falhar, reporte o erro e pare

## Regras para Modelos MoE
- Sem tabelas markdown: Use listas numeradas com pares chave-valor
- Caminhos de arquivo devem ter prefixo `data/`
- Nunca escreva código Python ou scripts — personifique o papel via comportamento
- Se uma ferramenta falhar, reporte no campo `erros` e não continue silenciosamente
