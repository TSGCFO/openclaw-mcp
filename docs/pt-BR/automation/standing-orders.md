---
source_url: https://docs.openclaw.ai/pt-BR/automation/standing-orders
title: "Ordens permanentes - OpenClaw"
---

[Pular para o conteúdo principal](https://docs.openclaw.ai/pt-BR/automation/standing-orders#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/pt-BR)

![BR](https://d3gk2c5xim1je2.cloudfront.net/flags/BR.svg)

Português (BR)

Pesquisar...

Ctrl K

Pesquisar...

Navigation

Automation and tasks

Ordens permanentes

[Get started](https://docs.openclaw.ai/pt-BR) [Install](https://docs.openclaw.ai/pt-BR/install) [Channels](https://docs.openclaw.ai/pt-BR/channels) [Agents](https://docs.openclaw.ai/pt-BR/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/pt-BR/tools) [Models](https://docs.openclaw.ai/pt-BR/providers) [Platforms](https://docs.openclaw.ai/pt-BR/platforms) [Gateway & Ops](https://docs.openclaw.ai/pt-BR/gateway) [Reference](https://docs.openclaw.ai/pt-BR/cli) [Help](https://docs.openclaw.ai/pt-BR/help)

Na página

- [Por que usar ordens permanentes](https://docs.openclaw.ai/pt-BR/automation/standing-orders#por-que-usar-ordens-permanentes)
- [Como elas funcionam](https://docs.openclaw.ai/pt-BR/automation/standing-orders#como-elas-funcionam)
- [Anatomia de uma ordem permanente](https://docs.openclaw.ai/pt-BR/automation/standing-orders#anatomia-de-uma-ordem-permanente)
- [Ordens permanentes mais tarefas Cron](https://docs.openclaw.ai/pt-BR/automation/standing-orders#ordens-permanentes-mais-tarefas-cron)
- [Exemplos](https://docs.openclaw.ai/pt-BR/automation/standing-orders#exemplos)
- [Exemplo 1: conteúdo e redes sociais (ciclo semanal)](https://docs.openclaw.ai/pt-BR/automation/standing-orders#exemplo-1-conte%C3%BAdo-e-redes-sociais-ciclo-semanal)
- [Exemplo 2: operações financeiras (acionadas por evento)](https://docs.openclaw.ai/pt-BR/automation/standing-orders#exemplo-2-opera%C3%A7%C3%B5es-financeiras-acionadas-por-evento)
- [Exemplo 3: monitoramento e alertas (contínuo)](https://docs.openclaw.ai/pt-BR/automation/standing-orders#exemplo-3-monitoramento-e-alertas-cont%C3%ADnuo)
- [Padrão executar-verificar-relatar](https://docs.openclaw.ai/pt-BR/automation/standing-orders#padr%C3%A3o-executar-verificar-relatar)
- [Arquitetura multiprograma](https://docs.openclaw.ai/pt-BR/automation/standing-orders#arquitetura-multiprograma)
- [Melhores práticas](https://docs.openclaw.ai/pt-BR/automation/standing-orders#melhores-pr%C3%A1ticas)
- [Faça](https://docs.openclaw.ai/pt-BR/automation/standing-orders#fa%C3%A7a)
- [Evite](https://docs.openclaw.ai/pt-BR/automation/standing-orders#evite)
- [Relacionados](https://docs.openclaw.ai/pt-BR/automation/standing-orders#relacionados)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Ordens permanentes concedem ao seu agente **autoridade operacional permanente** para programas definidos. Em vez de fornecer instruções de tarefa individuais a cada vez, você define programas com escopo, acionadores e regras de escalonamento claros — e o agente executa autonomamente dentro desses limites.Esta é a diferença entre dizer ao seu assistente “envie o relatório semanal” toda sexta-feira e conceder autoridade permanente: “Você é responsável pelo relatório semanal. Compile-o toda sexta-feira, envie-o e só escale se algo parecer errado.”

## [​](https://docs.openclaw.ai/pt-BR/automation/standing-orders\#por-que-usar-ordens-permanentes)  Por que usar ordens permanentes

**Sem ordens permanentes:**

- Você precisa solicitar ao agente cada tarefa
- O agente fica ocioso entre solicitações
- Trabalho rotineiro é esquecido ou atrasado
- Você se torna o gargalo

**Com ordens permanentes:**

- O agente executa autonomamente dentro de limites definidos
- Trabalho rotineiro acontece no cronograma sem solicitação
- Você só se envolve em exceções e aprovações
- O agente preenche o tempo ocioso de forma produtiva

## [​](https://docs.openclaw.ai/pt-BR/automation/standing-orders\#como-elas-funcionam)  Como elas funcionam

Ordens permanentes são definidas nos arquivos do seu [espaço de trabalho do agente](https://docs.openclaw.ai/pt-BR/concepts/agent-workspace). A abordagem recomendada é incluí-las diretamente em `AGENTS.md` (que é injetado automaticamente em todas as sessões), para que o agente sempre as tenha em contexto. Para configurações maiores, você também pode colocá-las em um arquivo dedicado, como `standing-orders.md`, e referenciá-lo a partir de `AGENTS.md`.Cada programa especifica:

1. **Escopo** — o que o agente está autorizado a fazer
2. **Acionadores** — quando executar (cronograma, evento ou condição)
3. **Portões de aprovação** — o que exige aprovação humana antes da ação
4. **Regras de escalonamento** — quando parar e pedir ajuda

O agente carrega essas instruções em todas as sessões por meio dos arquivos de inicialização do espaço de trabalho (consulte [Espaço de trabalho do agente](https://docs.openclaw.ai/pt-BR/concepts/agent-workspace) para ver a lista completa de arquivos injetados automaticamente) e as executa, combinadas com [tarefas Cron](https://docs.openclaw.ai/pt-BR/automation/cron-jobs) para imposição baseada em tempo.

Coloque ordens permanentes em `AGENTS.md` para garantir que elas sejam carregadas em todas as sessões. A inicialização do espaço de trabalho injeta automaticamente `AGENTS.md`, `SOUL.md`, `TOOLS.md`, `IDENTITY.md`, `USER.md`, `HEARTBEAT.md`, `BOOTSTRAP.md` e `MEMORY.md` — mas não arquivos arbitrários em subdiretórios.

## [​](https://docs.openclaw.ai/pt-BR/automation/standing-orders\#anatomia-de-uma-ordem-permanente)  Anatomia de uma ordem permanente

```
## Program: Weekly Status Report

**Authority:** Compile data, generate report, deliver to stakeholders
**Trigger:** Every Friday at 4 PM (enforced via cron job)
**Approval gate:** None for standard reports. Flag anomalies for human review.
**Escalation:** If data source is unavailable or metrics look unusual (>2σ from norm)

### Execution steps

1. Pull metrics from configured sources
2. Compare to prior week and targets
3. Generate report in Reports/weekly/YYYY-MM-DD.md
4. Deliver summary via configured channel
5. Log completion to Agent/Logs/

### What NOT to do

- Do not send reports to external parties
- Do not modify source data
- Do not skip delivery if metrics look bad — report accurately
```

## [​](https://docs.openclaw.ai/pt-BR/automation/standing-orders\#ordens-permanentes-mais-tarefas-cron)  Ordens permanentes mais tarefas Cron

Ordens permanentes definem **o que** o agente está autorizado a fazer. [Tarefas Cron](https://docs.openclaw.ai/pt-BR/automation/cron-jobs) definem **quando** isso acontece. Elas funcionam juntas:

```
Standing Order: "You own the daily inbox triage"
    ↓
Cron Job (8 AM daily): "Execute inbox triage per standing orders"
    ↓
Agent: Reads standing orders → executes steps → reports results
```

O prompt da tarefa Cron deve referenciar a ordem permanente em vez de duplicá-la:

```
openclaw cron add \
  --name daily-inbox-triage \
  --cron "0 8 * * 1-5" \
  --tz America/New_York \
  --timeout-seconds 300 \
  --announce \
  --channel bluebubbles \
  --to "+1XXXXXXXXXX" \
  --message "Execute daily inbox triage per standing orders. Check mail for new alerts. Parse, categorize, and persist each item. Report summary to owner. Escalate unknowns."
```

## [​](https://docs.openclaw.ai/pt-BR/automation/standing-orders\#exemplos)  Exemplos

### [​](https://docs.openclaw.ai/pt-BR/automation/standing-orders\#exemplo-1-conte%C3%BAdo-e-redes-sociais-ciclo-semanal)  Exemplo 1: conteúdo e redes sociais (ciclo semanal)

```
## Program: Content & Social Media

**Authority:** Draft content, schedule posts, compile engagement reports
**Approval gate:** All posts require owner review for first 30 days, then standing approval
**Trigger:** Weekly cycle (Monday review → mid-week drafts → Friday brief)

### Weekly cycle

- **Monday:** Review platform metrics and audience engagement
- **Tuesday–Thursday:** Draft social posts, create blog content
- **Friday:** Compile weekly marketing brief → deliver to owner

### Content rules

- Voice must match the brand (see SOUL.md or brand voice guide)
- Never identify as AI in public-facing content
- Include metrics when available
- Focus on value to audience, not self-promotion
```

### [​](https://docs.openclaw.ai/pt-BR/automation/standing-orders\#exemplo-2-opera%C3%A7%C3%B5es-financeiras-acionadas-por-evento)  Exemplo 2: operações financeiras (acionadas por evento)

```
## Program: Financial Processing

**Authority:** Process transaction data, generate reports, send summaries
**Approval gate:** None for analysis. Recommendations require owner approval.
**Trigger:** New data file detected OR scheduled monthly cycle

### When new data arrives

1. Detect new file in designated input directory
2. Parse and categorize all transactions
3. Compare against budget targets
4. Flag: unusual items, threshold breaches, new recurring charges
5. Generate report in designated output directory
6. Deliver summary to owner via configured channel

### Escalation rules

- Single item > $500: immediate alert
- Category > budget by 20%: flag in report
- Unrecognizable transaction: ask owner for categorization
- Failed processing after 2 retries: report failure, do not guess
```

### [​](https://docs.openclaw.ai/pt-BR/automation/standing-orders\#exemplo-3-monitoramento-e-alertas-cont%C3%ADnuo)  Exemplo 3: monitoramento e alertas (contínuo)

```
## Program: System Monitoring

**Authority:** Check system health, restart services, send alerts
**Approval gate:** Restart services automatically. Escalate if restart fails twice.
**Trigger:** Every heartbeat cycle

### Checks

- Service health endpoints responding
- Disk space above threshold
- Pending tasks not stale (>24 hours)
- Delivery channels operational

### Response matrix

| Condition        | Action                   | Escalate?                |
| ---------------- | ------------------------ | ------------------------ |
| Service down     | Restart automatically    | Only if restart fails 2x |
| Disk space < 10% | Alert owner              | Yes                      |
| Stale task > 24h | Remind owner             | No                       |
| Channel offline  | Log and retry next cycle | If offline > 2 hours     |
```

## [​](https://docs.openclaw.ai/pt-BR/automation/standing-orders\#padr%C3%A3o-executar-verificar-relatar)  Padrão executar-verificar-relatar

Ordens permanentes funcionam melhor quando combinadas com uma disciplina rigorosa de execução. Cada tarefa em uma ordem permanente deve seguir este ciclo:

1. **Executar** — Faça o trabalho real (não apenas reconheça a instrução)
2. **Verificar** — Confirme que o resultado está correto (o arquivo existe, a mensagem foi entregue, os dados foram analisados)
3. **Relatar** — Informe ao proprietário o que foi feito e o que foi verificado

```
### Execution rules

- Every task follows Execute-Verify-Report. No exceptions.
- "I'll do that" is not execution. Do it, then report.
- "Done" without verification is not acceptable. Prove it.
- If execution fails: retry once with adjusted approach.
- If still fails: report failure with diagnosis. Never silently fail.
- Never retry indefinitely — 3 attempts max, then escalate.
```

Esse padrão evita o modo de falha mais comum do agente: reconhecer uma tarefa sem concluí-la.

## [​](https://docs.openclaw.ai/pt-BR/automation/standing-orders\#arquitetura-multiprograma)  Arquitetura multiprograma

Para agentes que gerenciam várias áreas, organize ordens permanentes como programas separados com limites claros:

```
## Program 1: [Domain A] (Weekly)

...

## Program 2: [Domain B] (Monthly + On-Demand)

...

## Program 3: [Domain C] (As-Needed)

...

## Escalation Rules (All Programs)

- [Common escalation criteria]
- [Approval gates that apply across programs]
```

Cada programa deve ter:

- Sua própria **cadência de acionamento** (semanal, mensal, orientada por eventos, contínua)
- Seus próprios **portões de aprovação** (alguns programas precisam de mais supervisão do que outros)
- **Limites** claros (o agente deve saber onde um programa termina e outro começa)

## [​](https://docs.openclaw.ai/pt-BR/automation/standing-orders\#melhores-pr%C3%A1ticas)  Melhores práticas

### [​](https://docs.openclaw.ai/pt-BR/automation/standing-orders\#fa%C3%A7a)  Faça

- Comece com autoridade restrita e expanda conforme a confiança aumenta
- Defina portões de aprovação explícitos para ações de alto risco
- Inclua seções “O que NÃO fazer” — limites importam tanto quanto permissões
- Combine com tarefas Cron para execução confiável baseada em tempo
- Revise os logs do agente semanalmente para verificar se as ordens permanentes estão sendo seguidas
- Atualize as ordens permanentes conforme suas necessidades evoluem — elas são documentos vivos

### [​](https://docs.openclaw.ai/pt-BR/automation/standing-orders\#evite)  Evite

- Conceder autoridade ampla no primeiro dia (“faça o que você achar melhor”)
- Ignorar regras de escalonamento — todo programa precisa de uma cláusula de “quando parar e perguntar”
- Presumir que o agente lembrará instruções verbais — coloque tudo no arquivo
- Misturar assuntos em um único programa — programas separados para domínios separados
- Esquecer de impor com tarefas Cron — ordens permanentes sem acionadores viram sugestões

## [​](https://docs.openclaw.ai/pt-BR/automation/standing-orders\#relacionados)  Relacionados

- [Automação e tarefas](https://docs.openclaw.ai/pt-BR/automation): todos os mecanismos de automação em resumo.
- [Tarefas Cron](https://docs.openclaw.ai/pt-BR/automation/cron-jobs): imposição de cronograma para ordens permanentes.
- [Hooks](https://docs.openclaw.ai/pt-BR/automation/hooks): scripts orientados por eventos para eventos do ciclo de vida do agente.
- [Webhooks](https://docs.openclaw.ai/pt-BR/automation/cron-jobs#webhooks): acionadores de eventos HTTP de entrada.
- [Espaço de trabalho do agente](https://docs.openclaw.ai/pt-BR/concepts/agent-workspace): onde ficam as ordens permanentes, incluindo a lista completa de arquivos de inicialização injetados automaticamente (`AGENTS.md`, `SOUL.md` etc.).

[Fluxo de tarefas](https://docs.openclaw.ai/pt-BR/automation/taskflow) [Ganchos](https://docs.openclaw.ai/pt-BR/automation/hooks)

Ctrl+I