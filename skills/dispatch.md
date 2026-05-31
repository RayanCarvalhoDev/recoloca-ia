# Dispatch Skill — Delegação de Agentes

## Visão Geral
Skill do Maestro para delegar tarefas a agentes especializados usando a ferramenta `spawn_agent`.

## Ferramenta Utilizada
- `spawn_agent` — despacha um agente com uma tarefa específica

## Parâmetros do `spawn_agent`

- `label`: Nome amigável exibido na UI (ex: "Scout - Busca de Vagas")
- `message`: Prompt completo contendo o envelope de despacho
- `session_id`: (opcional) ID de sessão existente para continuar conversa com agente já despachado

## Construção do Envelope de Despacho

O Maestro deve construir um envelope contendo:

```
## DESPACHO: [NOME_DO_AGENTE]
### referencia_persona
[Conteúdo completo de personas/[agente].md]

### tarefa
[Descrição clara e objetiva do que deve ser feito]

### perfil_usuario
[Conteúdo de data/user-profile.md]

### contexto
[Variáveis relevantes: Area, Localizacao, Nivel, Habilidades, etc.]

### saida_esperada
[Formato esperado da resposta, ex: Envelope de resposta com estado, resumo, dados e erros]
```

## Fluxo de Despacho

1. **Identificar agente adequado** para a tarefa
2. **Ler a persona** do agente (`personas/[agente].md`)
3. **Ler perfil do usuário** (`data/user-profile.md`)
4. **Construir envelope** com todas as informações necessárias
5. **Chamar `spawn_agent`** com label e message
6. **Receber resposta** do agente
7. **Processar resultado** (salvar em `data/`, exibir ao usuário)
8. **Retornar ao menu** principal

## Tratamento de Erros

- **Falha no spawn_agent**: Informar erro ao usuário, retornar ao menu
- **Resposta com estado: erro**: Exibir erros reportados pelo agente, retornar ao menu
- **Timeout ou resposta incompleta**: Informar ao usuário, sugerir retry

## Regras para Modelos MoE

- Sempre inclua a persona completa no envelope (não apenas referência)
- Seja explícito sobre o que o agente deve fazer
- Especifique o formato exato de saída esperado
- Nunca invente dados ao construir o envelope
- Caminhos devem ter prefixo `data/`
