# Skill: Firecrawl CLI

## Objetivo
Fornecer instruções precisas para uso do Firecrawl CLI via terminal do Zed para busca e extração de dados web.

## Pré-requisitos
- Firecrawl instalado e configurado
- Variável de ambiente `FIRECRAWL_API_KEY` definida

## Comandos Disponíveis

### 1. Busca de Vagas (Search)
```bash
firecrawl search "termo de busca" --json
```
- Retorna JSON com: url, titulo, descricao, cargo para cada resultado
- Use aspas duplas no termo de busca
- O parâmetro `--json` é obrigatório para parsing estruturado

### 2. Extração de Detalhes (Scrape)
```bash
firecrawl scrape <url> --format markdown
```
- Extrai o conteúdo completo da página em formato markdown limpo
- Use em URLs individuais de vagas para obter descrição e requisitos completos
- Se a extração falhar ou expirar, use o título e descrição do resultado da busca

## Regras de Uso
1. Sempre execute comandos firecrawl via `terminal` do Zed
2. O diretório de execução deve ser a raiz do projeto (`recoloca-AI/`)
3. Se o comando retornar erro, reporte o erro exato e não continue silenciosamente
4. Para buscas de vagas, use o termo: `"vagas [area] [localizacao]"`
5. Priorize URLs de sites conhecidos: infojobs.com.br, vagas.com.br, indeed.com.br

## Tratamento de Erros
- **Erro de autenticação**: Verificar se `FIRECRAWL_API_KEY` está definida
- **Erro de timeout**: Tentar novamente com a mesma URL ou usar dados da busca inicial
- **Erro de rate limit**: Aguardar ou reportar erro ao usuário
- **Nunca inventar dados**: Se o firecrawl falhar consistentemente, usar fallback documentado

## Integração com Scout
O Scout deve:
1. Usar `firecrawl search` como método primário
2. Usar `firecrawl scrape` para detalhes das vagas
3. Se falhar, fazer fallback para `fetch` nativo do Zed
4. Reportar erros no envelope de resposta
