<div align="center">

# 🤖 Recoloca.IA

### Sistema Multiagente de Recolocação Profissional com Inteligência Artificial

[![Autor](https://img.shields.io/badge/Autor-Rayan%20Carvalho-00D4AA?style=for-the-badge&logo=github)](https://github.com/RayanCarvalhoDev)
[![Ano](https://img.shields.io/badge/Ano-2026-FF6B6B?style=for-the-badge)](#)
[![Imersão](https://img.shields.io/badge/Imers%C3%A3o%20IA-2026-6C63FF?style=for-the-badge)](#)
[![IA](https://img.shields.io/badge/Powered%20by-Agentes%20de%20IA-FFD93D?style=for-the-badge)](#)

<br/>

> *"É difícil chegar lá, mas o início é o princípio de tudo... sem o início não terá o conhecimento final."*
>
> — Rayan Carvalho

</div>

---

## 📖 Sobre o Projeto

O **Recoloca.IA** é um sistema de **múltiplos agentes de inteligência artificial** construído para auxiliar profissionais em sua jornada de recolocação no mercado de trabalho.

O sistema identifica vagas compatíveis com o perfil do usuário, aponta as habilidades em falta, recomenda cursos **gratuitos** para preencher essas lacunas e ainda simula uma entrevista de emprego para preparar o candidato.

Desenvolvido por **Rayan Carvalho** durante a **Imersão IA 2026**, o projeto foi construído explorando conceitos de sistemas multiagente, planejamento com IA e automação inteligente de fluxos de trabalho.

---

## ✨ O que o sistema faz

```
Você responde um quiz de perfil
        ↓
O Maestro entende quem você é
        ↓
O Scout varre a internet em busca de vagas compatíveis
        ↓
Identifica quais habilidades estão faltando para cada vaga
        ↓
O Curator busca cursos GRATUITOS para cobrir esses gaps
        ↓
O Coach simula uma entrevista e dá feedback completo
        ↓
Você entra na entrevista real preparado
```

---

## 🤖 Os Agentes

| Agente | Função |
|---|---|
| 🎼 **Maestro** | Orquestrador principal — gerencia o menu, coordena os outros agentes e consolida os resultados |
| 🔍 **Scout** | Varre portais de emprego (Indeed, Catho, InfoJobs, Vagas.com) e encontra vagas compatíveis com seu perfil |
| 📚 **Curator** | Busca cursos **preferencialmente gratuitos** para cobrir as habilidades que ainda faltam para as vagas encontradas |
| 🎤 **Coach** | Simula uma entrevista de emprego com linguagem formal, avalia suas respostas e dá recomendações detalhadas de melhoria |

---

## 🎮 Menu do Sistema

Após inicializar, o Maestro apresenta o seguinte menu:

```
A) Responder o quiz de perfil (ou atualizar respostas)
B) Buscar vagas de emprego
C) Buscar cursos para complementar habilidades
D) Simular entrevista de emprego
```

### Fluxo recomendado para novos usuários:
1. Selecione **A** → Responda o quiz para criar seu perfil
2. Selecione **B** → O Scout busca vagas compatíveis e lista os gaps de habilidades
3. Selecione **C** → O Curator recomenda cursos gratuitos para os gaps encontrados
4. Selecione **D** → O Coach simula a entrevista e dá feedback completo

---

## 🏗️ Arquitetura

```
┌──────────────────────────────────────────────────┐
│                    Usuário                       │
└────────────────────┬─────────────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────────────┐
│            MAESTRO  (Orquestrador)               │
│   Interface principal · Coordenação              │
│   Consolida e apresenta os resultados            │
└────────┬─────────────┬──────────────┬────────────┘
         │             │              │
         ▼             ▼              ▼
    ┌─────────┐  ┌──────────┐  ┌──────────────┐
    │  SCOUT  │  │ CURATOR  │  │    COACH     │
    │  Busca  │  │  Busca   │  │  Simulação   │
    │  Vagas  │  │  Cursos  │  │ Entrevistas  │
    └─────────┘  └──────────┘  └──────────────┘
```

Cada agente roda com sua própria **janela de contexto limpa**, o que garante respostas mais precisas e consistentes — especialmente com modelos mais acessíveis (arquitetura MoE).

---

## 📁 Estrutura de Arquivos

```
recoloca-AI/
├── AGENTS.md                    ← Ponto de entrada do sistema
├── plano.md                     ← Plano geral (Aula 1)
├── plano-aula-2.md              ← Plano do Scout (Aula 2)
├── plano-aula-3.md              ← Plano do Curator (Aula 3)
├── plano-entrevistador.md       ← Plano do Coach (Aula 4)
│
├── personas/
│   ├── maestro.md               ← Persona do Maestro (orquestrador)
│   ├── scout.md                 ← Persona do Scout (busca de vagas)
│   ├── curator.md               ← Persona do Curator (busca de cursos)
│   └── coach.md                 ← Persona do Coach (simulação de entrevistas)
│
├── skills/
│   ├── firecrawl.md             ← Comandos e regras do FireCrawl
│   ├── dispatch.md              ← Como o Maestro despacha agentes
│   ├── job-search.md            ← Fluxo completo de busca de vagas
│   ├── course-search.md         ← Fluxo de busca de cursos gratuitos
│   └── interview-simulation.md  ← Fluxo da simulação de entrevista
│
└── data/
    ├── user-profile.md          ← Perfil do usuário (gerado pelo quiz)
    ├── job-search-results.md    ← Últimas vagas encontradas pelo Scout
    └── course-search-results.md ← Últimos cursos encontrados pelo Curator
```

---

## 🛠️ Pré-requisitos

Antes de rodar o projeto, você precisa ter instalado:

### 1. Zed Editor
O editor de código com suporte nativo a agentes de IA.

🔗 Download: [zed.dev](https://zed.dev)

---

### 2. OpenRouter (API Key)
O Zed utiliza o **OpenRouter** para acessar os modelos de IA. Você precisará criar uma conta e configurar sua própria chave de API.

🔗 Crie sua conta em: [openrouter.ai](https://openrouter.ai)

Após criar a conta:
1. Acesse **Keys** no painel do OpenRouter
2. Clique em **Create Key** e copie sua chave
3. No Zed, pressione `ALT + SHIFT + C` para abrir o **Agent Panel**
4. Clique em **Settings** dentro do painel e cole sua key do OpenRouter no campo indicado

> ⚠️ Nunca compartilhe sua API Key publicamente — ela dá acesso à sua conta e pode gerar custos.
>
> 🤡 E não, a minha key não vai estar aqui. Tentou, né? Vai criar a sua no [openrouter.ai](https://openrouter.ai) — é rapidinho, prometo que não dói.

#### 🆓 Use modelos gratuitos para não gastar créditos

No seletor de modelos do Zed, prefira modelos com `:free` no nome para evitar custos inesperados:

- `google/gemma-3-27b-it:free`
- `meta-llama/llama-3.3-70b-instruct:free`
- `mistralai/mistral-7b-instruct:free`
- `deepseek/deepseek-chat:free`

> ⚠️ Modelos **sem** `:free` no nome consomem créditos pagos. Fique de olho no modelo selecionado antes de iniciar uma conversa com o agente.

#### 🔄 Como rotacionar (trocar) sua Key — recomendado periodicamente

Se você suspeitar que sua key foi exposta ou quiser renovar por segurança:

1. Acesse [openrouter.ai/keys](https://openrouter.ai/keys)
2. Clique nos **três pontos** ao lado da sua key atual → **Delete**
3. Clique em **Create Key** para gerar uma nova
4. No Zed, vá em **Settings → AI Settings** e substitua pela nova key

#### 🗑️ Como remover sua Key do Zed

Caso queira desconectar o Zed do OpenRouter:

1. No Zed, abra as configurações com `CTRL + ,` (Windows/Linux) ou `CMD + ,` (macOS)
2. Vá em **AI Settings**
3. Apague o campo da API Key e salve

> 💡 A key fica salva apenas nas **configurações locais do Zed**, nunca nos arquivos do projeto — então subir o repositório para o GitHub não expõe sua chave.

---

### 3. Node.js (via NVM)
Necessário para instalar o FireCrawl.

**Windows** — Baixe o NVM para Windows:
🔗 [github.com/coreybutler/nvm-windows](https://github.com/coreybutler/nvm-windows)

Após instalar, abra o **PowerShell** e rode:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
nvm install latest
nvm use 26.2.0
```

**macOS/Linux:**
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install --lts
nvm use --lts
```

Verifique a instalação:
```bash
node --version
```

---

### 4. FireCrawl CLI
Ferramenta que permite ao Scout navegar em sites de vagas sem ser bloqueado por mecanismos anti-bot.

Crie uma conta gratuita em [firecrawl.dev](https://www.firecrawl.dev/) e obtenha sua **API Key**. Depois instale via terminal:

```bash
npx -y firecrawl-cli@latest init --all -k SUA_CHAVE_API_AQUI
```

> 💡 O plano gratuito do FireCrawl é suficiente para uso do projeto.

---

## 🚀 Como baixar e usar

### 1. Clone o repositório

```bash
git clone https://github.com/RayanCarvalhoDev/recoloca-ia.git
```

### 2. Acesse a pasta do projeto

```bash
cd recoloca-ia
```

### 3. Abra no Zed

```bash
zed .
```

### 4. Inicie uma nova conversa com o agente

No Zed, pressione `CTRL + ALT + J`, clique em `+` e selecione **Zed Agent**.

### 5. Inicialize o sistema

Digite o comando abaixo e pressione Enter:

```
Leia e siga o @AGENTS.md
```

O **Maestro** vai carregar e apresentar o menu principal. A partir daí, é só interagir!

---

## 💡 Conceitos explorados

- **Sistemas Multiagente** — múltiplos agentes especializados cooperando
- **Janela de Contexto** — por que agentes especializados são mais eficientes
- **MoE (Mixture of Experts)** — como modelos baratos funcionam internamente
- **Tool Calling** — como especificar ferramentas para o agente usar
- **Greenfield vs. Brownfield** — desenvolvimento do zero vs. em cima do que existe
- **AI FinOps** — gestão de custos em soluções com IA
- **Guardrails** — como proteger o comportamento do agente com linguagem natural

---

## 📦 Tecnologias

| Tecnologia | Uso |
|---|---|
| [Zed Editor](https://zed.dev) | IDE com suporte nativo a agentes de IA |
| [FireCrawl](https://firecrawl.dev) | Navegação web sem bloqueio para o Scout e Curator |
| [Node.js](https://nodejs.org) | Runtime necessário para o FireCrawl CLI |
| Markdown (`.md`) | Planos, personas, skills e dados do sistema |

---

## 👤 Autor

<div align="center">

**Rayan Carvalho**

Desenvolvido com 🤖 e ☕ durante a Imersão IA 2026

[![GitHub](https://img.shields.io/badge/GitHub-RayanCarvalhoDev-181717?style=for-the-badge&logo=github)](https://github.com/RayanCarvalhoDev)

</div>

---

## 📄 Licença

Este projeto foi desenvolvido para fins educacionais durante a Imersão IA 2026.

---

<div align="center">

*"É difícil chegar lá, mas o início é o princípio de tudo... sem o início não terá o conhecimento final."*

</div>
