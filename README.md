# Enterprise Infrastructure Homelab

> Laboratório prático de Infraestrutura de TI, Linux, virtualização, redes e containers, com evolução planejada para observabilidade, automação e Cloud.

## Visão geral

Este repositório documenta a construção progressiva de um ambiente de estudo com servidor físico, **Proxmox VE**, **Ubuntu Server** e **Docker**. O objetivo é demonstrar competências práticas de administração de sistemas, troubleshooting e documentação técnica.

**Status:** em desenvolvimento. **Última etapa registrada (13/11/2026):** Ansible Core 2.16.3 configurado para automatizar a instalação do Nginx em uma segunda VM Ubuntu Server, com teste HTTP 200 OK. Os procedimentos aqui registrados refletem o laboratório de estudo; resultados e evidências adicionais serão incorporados conforme as validações.

## Arquitetura atual (simplificada)

```text
Servidor físico
└── Proxmox VE (hypervisor)
    └── VM Ubuntu Server
        ├── Rede TCP/IP e SSH
        ├── Administração Linux (usuários, grupos, permissões)
        ├── Serviço systemd e logs
        └── Docker + Docker Compose
            ├── Nginx (proxy reverso)
            ├── Flask (API Python)
            └── PostgreSQL (banco e volume)


    └── Ansible Core 2.16.3 (nó de controle)
        └── SSH → segunda VM Ubuntu Server
            └── Nginx 1.24.0 (HTTP 200 OK)

Histórico anterior: Nginx e Apache em containers independentes (outubro/2026).
```

> Diagrama lógico: não representa todos os equipamentos, portas ou segmentos de rede. Endereços IP, dados corporativos e credenciais foram omitidos deliberadamente.

## Tecnologias

- **Virtualização:** Proxmox VE, máquinas virtuais
- **Sistemas operacionais:** Ubuntu Server, Linux
- **Rede:** IPv4, gateway, DNS, SSH
- **Administração Linux:** contas, grupos, permissões, systemd e journalctl
- **Containers:** Docker, Docker Compose, Nginx, Apache (experimento anterior), Flask e PostgreSQL
- **Automação de configuração:** Ansible Core, inventário, playbooks, SSH por chave e Nginx remoto
- **Próximos estudos:** monitoramento, Terraform e integração AWS (ainda não validados neste projeto)

## Documentação

| Documento | Conteúdo |
| --- | --- |
| [Visão geral do ambiente](docs/01-ambiente.md) | Componentes, escopo e arquitetura |
| [Administração Linux e SSH](docs/02-linux-ssh.md) | Comandos explicados e verificações |
| [Docker e Compose](docs/03-docker-compose.md) | Serviços e persistência de dados |
| [Roadmap](docs/04-roadmap.md) | Implementações atuais e planejadas |
| [Proxmox e Ubuntu Server](docs/05-proxmox-ubuntu.md) | Inventário, topologia, comandos e validações pendentes |
| [Guia de evidências](docs/06-evidencias.md) | Checklist de capturas e cuidados com dados sensíveis |
| [Testes de conectividade e HTTP](docs/07-validacao-conectividade-containers.md) | Ping, DNS e respostas HTTP dos containers |
| [Aplicação Flask + PostgreSQL + Nginx](docs/08-flask-postgresql-nginx.md) | Arquitetura multicontainer, testes e próximos passos |
| [Ansible e Nginx remoto](docs/09-ansible-nginx.md) | Automação validada em segunda VM Ubuntu e HTTP 200 OK |

## Como ler este projeto

Cada documento separa **atividade realizada**, **verificação** e **próximas atividades**. Comandos são acompanhados de explicações, mas devem ser adaptados ao próprio ambiente antes de executar.

## Evidências e segurança

As capturas de tela e logs serão adicionados em `evidencias/` após revisão. **Não publicar** senhas, tokens, chaves SSH privadas, arquivos `.env`, IPs corporativos, números de série nem dados de terceiros. Configurações compartilhadas devem usar valores fictícios.

## Autor

Projeto de estudos e portfólio técnico de **Matheus Silvestre de Oliveira Dias**.

---

*Documentation in Portuguese. English summary: A practical infrastructure homelab featuring Proxmox, Ubuntu Server, Linux administration, SSH, Docker Compose, Flask, PostgreSQL and persistent volumes; Ansible automation across two Ubuntu VMs; future phases include monitoring, Terraform and AWS.*
