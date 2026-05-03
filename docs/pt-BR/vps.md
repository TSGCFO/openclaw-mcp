---
source_url: https://docs.openclaw.ai/pt-BR/vps
title: "Servidor Linux - OpenClaw"
---

[Pular para o conteúdo principal](https://docs.openclaw.ai/pt-BR/vps#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/pt-BR)

![BR](https://d3gk2c5xim1je2.cloudfront.net/flags/BR.svg)

Português (BR)

Pesquisar...

Ctrl K

Pesquisar...

Navigation

Hosting

Servidor Linux

[Get started](https://docs.openclaw.ai/pt-BR) [Install](https://docs.openclaw.ai/pt-BR/install) [Channels](https://docs.openclaw.ai/pt-BR/channels) [Agents](https://docs.openclaw.ai/pt-BR/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/pt-BR/tools) [Models](https://docs.openclaw.ai/pt-BR/providers) [Platforms](https://docs.openclaw.ai/pt-BR/platforms) [Gateway & Ops](https://docs.openclaw.ai/pt-BR/gateway) [Reference](https://docs.openclaw.ai/pt-BR/cli) [Help](https://docs.openclaw.ai/pt-BR/help)

Na página

- [Escolha um provedor](https://docs.openclaw.ai/pt-BR/vps#escolha-um-provedor)
- [Como as configurações em nuvem funcionam](https://docs.openclaw.ai/pt-BR/vps#como-as-configura%C3%A7%C3%B5es-em-nuvem-funcionam)
- [Proteja o acesso administrativo primeiro](https://docs.openclaw.ai/pt-BR/vps#proteja-o-acesso-administrativo-primeiro)
- [Agente compartilhado da empresa em uma VPS](https://docs.openclaw.ai/pt-BR/vps#agente-compartilhado-da-empresa-em-uma-vps)
- [Usando nodes com uma VPS](https://docs.openclaw.ai/pt-BR/vps#usando-nodes-com-uma-vps)
- [Ajuste de inicialização para VMs pequenas e hosts ARM](https://docs.openclaw.ai/pt-BR/vps#ajuste-de-inicializa%C3%A7%C3%A3o-para-vms-pequenas-e-hosts-arm)
- [Lista de verificação de ajuste do systemd (opcional)](https://docs.openclaw.ai/pt-BR/vps#lista-de-verifica%C3%A7%C3%A3o-de-ajuste-do-systemd-opcional)
- [Relacionado](https://docs.openclaw.ai/pt-BR/vps#relacionado)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Execute o Gateway do OpenClaw em qualquer servidor Linux ou VPS em nuvem. Esta página ajuda você a
escolher um provedor, explica como implantações em nuvem funcionam e cobre ajustes
genéricos de Linux que se aplicam em todos os lugares.

## [​](https://docs.openclaw.ai/pt-BR/vps\#escolha-um-provedor)  Escolha um provedor

[**Railway** \\
\\
Configuração no navegador com um clique](https://docs.openclaw.ai/pt-BR/install/railway)

[**Northflank** \\
\\
Configuração no navegador com um clique](https://docs.openclaw.ai/pt-BR/install/northflank)

[**DigitalOcean** \\
\\
VPS pago simples](https://docs.openclaw.ai/pt-BR/install/digitalocean)

[**Oracle Cloud** \\
\\
Nível ARM Always Free](https://docs.openclaw.ai/pt-BR/install/oracle)

[**Fly.io** \\
\\
Fly Machines](https://docs.openclaw.ai/pt-BR/install/fly)

[**Hetzner** \\
\\
Docker em VPS Hetzner](https://docs.openclaw.ai/pt-BR/install/hetzner)

[**Hostinger** \\
\\
VPS com configuração em um clique](https://docs.openclaw.ai/pt-BR/install/hostinger)

[**GCP** \\
\\
Compute Engine](https://docs.openclaw.ai/pt-BR/install/gcp)

[**Azure** \\
\\
VM Linux](https://docs.openclaw.ai/pt-BR/install/azure)

[**exe.dev** \\
\\
VM com proxy HTTPS](https://docs.openclaw.ai/pt-BR/install/exe-dev)

[**Raspberry Pi** \\
\\
ARM auto-hospedado](https://docs.openclaw.ai/pt-BR/install/raspberry-pi)

**AWS (EC2 / Lightsail / nível gratuito)** também funciona bem.
Um vídeo passo a passo da comunidade está disponível em
[x.com/techfrenAJ/status/2014934471095812547](https://x.com/techfrenAJ/status/2014934471095812547)
(recurso da comunidade — pode ficar indisponível).

## [​](https://docs.openclaw.ai/pt-BR/vps\#como-as-configura%C3%A7%C3%B5es-em-nuvem-funcionam)  Como as configurações em nuvem funcionam

- O **Gateway é executado na VPS** e é dono do estado + workspace.
- Você se conecta do seu laptop ou telefone pela **Interface de Controle** ou por **Tailscale/SSH**.
- Trate a VPS como a fonte da verdade e **faça backup** do estado + workspace regularmente.
- Padrão seguro: mantenha o Gateway em loopback e acesse-o por túnel SSH ou Tailscale Serve.
Se você fizer bind em `lan` ou `tailnet`, exija `gateway.auth.token` ou `gateway.auth.password`.

Páginas relacionadas: [acesso remoto ao Gateway](https://docs.openclaw.ai/pt-BR/gateway/remote), [hub de plataformas](https://docs.openclaw.ai/pt-BR/platforms).

## [​](https://docs.openclaw.ai/pt-BR/vps\#proteja-o-acesso-administrativo-primeiro)  Proteja o acesso administrativo primeiro

Antes de instalar o OpenClaw em uma VPS pública, decida como você quer administrar
a própria máquina.

- Se você quer acesso administrativo apenas pela tailnet, instale o Tailscale primeiro, adicione a VPS
à sua tailnet, verifique uma segunda sessão SSH pelo IP do Tailscale ou
nome MagicDNS e então restrinja o SSH público.
- Se você não estiver usando Tailscale, aplique o endurecimento equivalente ao seu caminho
SSH antes de expor mais serviços.
- Isso é separado do acesso ao Gateway. Você ainda pode manter o OpenClaw vinculado a
loopback e usar um túnel SSH ou Tailscale Serve para o painel.

As opções de Gateway específicas do Tailscale ficam em [Tailscale](https://docs.openclaw.ai/pt-BR/gateway/tailscale).

## [​](https://docs.openclaw.ai/pt-BR/vps\#agente-compartilhado-da-empresa-em-uma-vps)  Agente compartilhado da empresa em uma VPS

Executar um único agente para uma equipe é uma configuração válida quando todos os usuários estão no mesmo limite de confiança e o agente é apenas para uso empresarial.

- Mantenha-o em um runtime dedicado (VPS/VM/contêiner + usuário/contas de SO dedicados).
- Não faça login nesse runtime com contas pessoais da Apple/Google nem perfis pessoais de navegador/gerenciador de senhas.
- Se os usuários forem adversariais entre si, separe por gateway/host/usuário do SO.

Detalhes do modelo de segurança: [Segurança](https://docs.openclaw.ai/pt-BR/gateway/security).

## [​](https://docs.openclaw.ai/pt-BR/vps\#usando-nodes-com-uma-vps)  Usando nodes com uma VPS

Você pode manter o Gateway na nuvem e parear **nodes** nos seus dispositivos locais
(Mac/iOS/Android/headless). Nodes fornecem recursos locais de tela/câmera/canvas e `system.run`
enquanto o Gateway permanece na nuvem.Documentação: [Nodes](https://docs.openclaw.ai/pt-BR/nodes), [CLI de Nodes](https://docs.openclaw.ai/pt-BR/cli/nodes).

## [​](https://docs.openclaw.ai/pt-BR/vps\#ajuste-de-inicializa%C3%A7%C3%A3o-para-vms-pequenas-e-hosts-arm)  Ajuste de inicialização para VMs pequenas e hosts ARM

Se comandos da CLI parecerem lentos em VMs de baixa potência (ou hosts ARM), habilite o cache de compilação de módulos do Node:

```
grep -q 'NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache' ~/.bashrc || cat >> ~/.bashrc <<'EOF'
export NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache
mkdir -p /var/tmp/openclaw-compile-cache
export OPENCLAW_NO_RESPAWN=1
EOF
source ~/.bashrc
```

- `NODE_COMPILE_CACHE` melhora os tempos de inicialização em execuções repetidas de comandos.
- `OPENCLAW_NO_RESPAWN=1` evita sobrecarga extra de inicialização de um caminho de autorrespawn.
- A primeira execução de comando aquece o cache; as execuções posteriores são mais rápidas.
- Para especificidades do Raspberry Pi, consulte [Raspberry Pi](https://docs.openclaw.ai/pt-BR/install/raspberry-pi).

### [​](https://docs.openclaw.ai/pt-BR/vps\#lista-de-verifica%C3%A7%C3%A3o-de-ajuste-do-systemd-opcional)  Lista de verificação de ajuste do systemd (opcional)

Para hosts de VM que usam `systemd`, considere:

- Adicionar env de serviço para um caminho de inicialização estável:
  - `OPENCLAW_NO_RESPAWN=1`
  - `NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache`
- Manter o comportamento de reinicialização explícito:
  - `Restart=always`
  - `RestartSec=2`
  - `TimeoutStartSec=90`
- Prefira discos com SSD para caminhos de estado/cache a fim de reduzir penalidades de inicialização a frio por E/S aleatória.

Para o caminho padrão `openclaw onboard --install-daemon`, edite a unit de usuário:

```
systemctl --user edit openclaw-gateway.service
```

```
[Service]
Environment=OPENCLAW_NO_RESPAWN=1
Environment=NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache
Restart=always
RestartSec=2
TimeoutStartSec=90
```

Se você instalou deliberadamente uma unit de sistema em vez disso, edite
`openclaw-gateway.service` via `sudo systemctl edit openclaw-gateway.service`.Como políticas `Restart=` ajudam na recuperação automatizada:
[systemd pode automatizar a recuperação de serviços](https://www.redhat.com/en/blog/systemd-automate-recovery).Para comportamento de OOM no Linux, seleção de vítima de processo filho e diagnósticos de `exit 137`,
consulte [pressão de memória no Linux e encerramentos por OOM](https://docs.openclaw.ai/pt-BR/platforms/linux#memory-pressure-and-oom-kills).

## [​](https://docs.openclaw.ai/pt-BR/vps\#relacionado)  Relacionado

- [Visão geral da instalação](https://docs.openclaw.ai/pt-BR/install)
- [DigitalOcean](https://docs.openclaw.ai/pt-BR/install/digitalocean)
- [Fly.io](https://docs.openclaw.ai/pt-BR/install/fly)
- [Hetzner](https://docs.openclaw.ai/pt-BR/install/hetzner)

[Kubernetes](https://docs.openclaw.ai/pt-BR/install/kubernetes) [Máquinas virtuais do macOS](https://docs.openclaw.ai/pt-BR/install/macos-vm)

Ctrl+I