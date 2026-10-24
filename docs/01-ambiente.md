# 01 — Ambiente e arquitetura

## Objetivo

Montar um laboratório de infraestrutura para aprender instalação, operação, diagnóstico e documentação de serviços.

## Componentes trabalhados

- Servidor físico com Proxmox VE como hipervisor.
- Máquina virtual Ubuntu Server.
- Conectividade IPv4 e acesso remoto SSH.
- Serviços Linux, Docker e Docker Compose.

## Fluxo lógico

```text
Hardware físico
   |
Proxmox VE
   |
Ubuntu Server (VM)
   +-- SSH e administração remota
   +-- systemd / logs
   +-- Docker Engine
       +-- Nginx
       +-- Apache
       +-- volumes locais
```

## O que já foi praticado

1. Instalação e acesso ao Proxmox.
2. Implantação da VM Ubuntu Server.
3. Configuração e diagnóstico de rede e DNS.
4. Gerenciamento de usuários, grupos e permissões Linux.
5. Acesso SSH e operação de serviços systemd.
6. Containers e teste de persistência de volume com Compose.

## Próximas evidências

- Captura anonimizada do inventário do host e das VMs.
- Diagrama detalhado de interfaces e redes, sem expor endereços de empresa.
- Prints de testes de acesso e serviços em funcionamento.
