---
source_url: https://docs.openclaw.ai/pt-BR/start/wizard
title: "Integra\u00e7\u00e3o (CLI) - OpenClaw"
---

[Pular para o conteúdo principal](https://docs.openclaw.ai/pt-BR/start/wizard#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/pt-BR)

![BR](https://d3gk2c5xim1je2.cloudfront.net/flags/BR.svg)

Português (BR)

Pesquisar...

Ctrl K

Pesquisar...

Navigation

First steps

Integração (CLI)

[Get started](https://docs.openclaw.ai/pt-BR) [Install](https://docs.openclaw.ai/pt-BR/install) [Channels](https://docs.openclaw.ai/pt-BR/channels) [Agents](https://docs.openclaw.ai/pt-BR/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/pt-BR/tools) [Models](https://docs.openclaw.ai/pt-BR/providers) [Platforms](https://docs.openclaw.ai/pt-BR/platforms) [Gateway & Ops](https://docs.openclaw.ai/pt-BR/gateway) [Reference](https://docs.openclaw.ai/pt-BR/cli) [Help](https://docs.openclaw.ai/pt-BR/help)

Na página

- [Início rápido vs Avançado](https://docs.openclaw.ai/pt-BR/start/wizard#in%C3%ADcio-r%C3%A1pido-vs-avan%C3%A7ado)
- [O que a integração configura](https://docs.openclaw.ai/pt-BR/start/wizard#o-que-a-integra%C3%A7%C3%A3o-configura)
- [Adicionar outro agente](https://docs.openclaw.ai/pt-BR/start/wizard#adicionar-outro-agente)
- [Referência completa](https://docs.openclaw.ai/pt-BR/start/wizard#refer%C3%AAncia-completa)
- [Docs relacionados](https://docs.openclaw.ai/pt-BR/start/wizard#docs-relacionados)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

A integração via CLI é a forma **recomendada** de configurar o OpenClaw no macOS,
Linux ou Windows (via WSL2; fortemente recomendado).
Ela configura um Gateway local ou uma conexão com Gateway remoto, além de canais, skills
e padrões de workspace em um único fluxo guiado.

```
openclaw onboard
```

Primeiro chat mais rápido: abra a Control UI (nenhuma configuração de canal necessária). Execute
`openclaw dashboard` e converse no navegador. Docs: [Dashboard](https://docs.openclaw.ai/pt-BR/web/dashboard).

Para reconfigurar depois:

```
openclaw configure
openclaw agents add <name>
```

`--json` não implica modo não interativo. Para scripts, use `--non-interactive`.

A integração via CLI inclui uma etapa de busca na web em que você pode escolher um provedor
como Brave, DuckDuckGo, Exa, Firecrawl, Gemini, Grok, Kimi, MiniMax Search,
Ollama Web Search, Perplexity, SearXNG ou Tavily. Alguns provedores exigem uma
chave de API, enquanto outros não precisam de chave. Você também pode configurar isso depois com
`openclaw configure --section web`. Docs: [Ferramentas web](https://docs.openclaw.ai/pt-BR/tools/web).

## [​](https://docs.openclaw.ai/pt-BR/start/wizard\#in%C3%ADcio-r%C3%A1pido-vs-avan%C3%A7ado)  Início rápido vs Avançado

A integração começa com **Início rápido** (padrões) vs **Avançado** (controle total).

- Início rápido (padrões)

- Avançado (controle total)


- Gateway local (loopback)
- Padrão de workspace (ou workspace existente)
- Porta do Gateway **18789**
- Autenticação do Gateway **Token** (gerado automaticamente, mesmo em loopback)
- Política de ferramentas padrão para novas configurações locais: `tools.profile: "coding"` (o perfil explícito existente é preservado)
- Padrão de isolamento de MD: a integração local grava `session.dmScope: "per-channel-peer"` quando não definido. Detalhes: [Referência de configuração da CLI](https://docs.openclaw.ai/pt-BR/start/wizard-cli-reference#outputs-and-internals)
- Exposição por Tailscale **Desativada**
- MDs do Telegram + WhatsApp usam **lista de permissões** por padrão (você será solicitado a informar seu número de telefone)

- Expõe todas as etapas (modo, workspace, gateway, canais, daemon, skills).

## [​](https://docs.openclaw.ai/pt-BR/start/wizard\#o-que-a-integra%C3%A7%C3%A3o-configura)  O que a integração configura

O **modo local (padrão)** guia você por estas etapas:

1. **Modelo/Auth** — escolha qualquer provedor/fluxo de autenticação compatível (chave de API, OAuth ou autenticação manual específica do provedor), incluindo Custom Provider
(compatível com OpenAI, compatível com Anthropic ou detecção automática Unknown). Escolha um modelo padrão.
Nota de segurança: se este agente for executar ferramentas ou processar conteúdo de webhook/hooks, prefira o modelo mais forte de última geração disponível e mantenha a política de ferramentas restrita. Camadas mais fracas/antigas são mais fáceis de sofrer prompt injection.
Para execuções não interativas, `--secret-input-mode ref` armazena referências baseadas em env nos perfis de autenticação em vez de valores de chave de API em texto simples.
No modo `ref` não interativo, a variável de ambiente do provedor deve estar definida; passar flags de chave inline sem essa variável de ambiente falha rapidamente.
Em execuções interativas, escolher o modo de referência secreta permite apontar para uma variável de ambiente ou uma referência de provedor configurada (`file` ou `exec`), com uma validação rápida de preflight antes de salvar.
Para Anthropic, onboarding/configure interativo oferece **Anthropic Claude CLI** como o caminho local preferido e **chave de API da Anthropic** como o caminho de produção recomendado. Anthropic setup-token também continua disponível como um caminho de autenticação por token compatível.
2. **Workspace** — local dos arquivos do agente (padrão `~/.openclaw/workspace`). Semeia arquivos de bootstrap.
3. **Gateway** — porta, endereço de bind, modo de autenticação, exposição por Tailscale.
No modo de token interativo, escolha o armazenamento padrão de token em texto simples ou opte por SecretRef.
Caminho SecretRef de token não interativo: `--gateway-token-ref-env <ENV_VAR>`.
4. **Canais** — canais de chat integrados e empacotados, como BlueBubbles, Discord, Feishu, Google Chat, Mattermost, Microsoft Teams, QQ Bot, Signal, Slack, Telegram, WhatsApp e outros.
5. **Daemon** — instala um LaunchAgent (macOS), uma unidade de usuário systemd (Linux/WSL2) ou uma Tarefa Agendada nativa do Windows com fallback para a pasta Inicializar por usuário.
Se a autenticação por token exigir um token e `gateway.auth.token` for gerenciado por SecretRef, a instalação do daemon o valida, mas não persiste o token resolvido nos metadados de ambiente do serviço supervisor.
Se a autenticação por token exigir um token e o SecretRef de token configurado não for resolvido, a instalação do daemon é bloqueada com orientação acionável.
Se `gateway.auth.token` e `gateway.auth.password` estiverem configurados e `gateway.auth.mode` não estiver definido, a instalação do daemon é bloqueada até que o modo seja definido explicitamente.
6. **Verificação de integridade** — inicia o Gateway e verifica se ele está em execução.
7. **Skills** — instala skills recomendadas e dependências opcionais.

Executar a integração novamente **não** apaga nada, a menos que você escolha explicitamente **Redefinir** (ou passe `--reset`).
Por padrão, `--reset` da CLI redefine configuração, credenciais e sessões; use `--reset-scope full` para incluir o workspace.
Se a configuração for inválida ou contiver chaves legadas, a integração solicitará que você execute `openclaw doctor` primeiro.

O **modo remoto** configura apenas o cliente local para se conectar a um Gateway em outro lugar.
Ele **não** instala nem altera nada no host remoto.

## [​](https://docs.openclaw.ai/pt-BR/start/wizard\#adicionar-outro-agente)  Adicionar outro agente

Use `openclaw agents add <name>` para criar um agente separado com seu próprio workspace,
sessões e perfis de autenticação. Executar sem `--workspace` inicia a integração.O que ele define:

- `agents.list[].name`
- `agents.list[].workspace`
- `agents.list[].agentDir`

Notas:

- Workspaces padrão seguem `~/.openclaw/workspace-<agentId>`.
- Adicione `bindings` para rotear mensagens recebidas (a integração pode fazer isso).
- Flags não interativas: `--model`, `--agent-dir`, `--bind`, `--non-interactive`.

## [​](https://docs.openclaw.ai/pt-BR/start/wizard\#refer%C3%AAncia-completa)  Referência completa

Para detalhamentos passo a passo e saídas de configuração, consulte
[Referência de configuração da CLI](https://docs.openclaw.ai/pt-BR/start/wizard-cli-reference).
Para exemplos não interativos, consulte [Automação da CLI](https://docs.openclaw.ai/pt-BR/start/wizard-cli-automation).
Para a referência técnica mais aprofundada, incluindo detalhes de RPC, consulte
[Referência de integração](https://docs.openclaw.ai/pt-BR/reference/wizard).

## [​](https://docs.openclaw.ai/pt-BR/start/wizard\#docs-relacionados)  Docs relacionados

- Referência de comandos da CLI: [`openclaw onboard`](https://docs.openclaw.ai/pt-BR/cli/onboard)
- Visão geral da integração: [Visão geral da integração](https://docs.openclaw.ai/pt-BR/start/onboarding-overview)
- Integração do app macOS: [Integração](https://docs.openclaw.ai/pt-BR/start/onboarding)
- Ritual de primeira execução do agente: [Bootstrap do agente](https://docs.openclaw.ai/pt-BR/start/bootstrapping)

[Onboarding Overview](https://docs.openclaw.ai/pt-BR/start/onboarding-overview) [Onboarding: macOS App](https://docs.openclaw.ai/pt-BR/start/onboarding)

Ctrl+I