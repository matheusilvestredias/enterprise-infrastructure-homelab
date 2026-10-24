# Enterprise Infrastructure Homelab

> Laboratório prático de Infraestrutura de TI, Linux, virtualização, redes e containers, com evolução planejada para observabilidade, automação e Cloud.

## Visão geral

Este repositório documenta a construção progressiva de um ambiente de estudo com servidor físico, **Proxmox VE**, **Ubuntu Server** e **Docker**. O objetivo é demonstrar competências práticas de administração de sistemas, troubleshooting e documentação técnica.

**Status:** em desenvolvimento. Os procedimentos aqui registrados refletem o laboratório de estudo; resultados e evidências adicionais serão incorporados conforme as validações.

## Arquitetura atual (simplificada)

```text
Servidor físico
└── Proxmox VE (hypervisor)
    └── VM Ubuntu Server
        ├── Rede TCP/IP e SSH
        ├── Administração Linux (usuários, grupos, permissões)
        ├── Serviço systemd e logs
        └── Docker + Docker Compose
            ├── Nginx
            ├── Apache
            └── Volumes persistentes
```

> Diagrama lógico: não representa todos os equipamentos, portas ou segmentos de rede. Endereços IP, dados corporativos e credenciais foram omitidos deliberadamente.

## Tecnologias

- **Virtualização:** Proxmox VE, máquinas virtuais
- **Sistemas operacionais:** Ubuntu Server, Linux
- **Rede:** IPv4, gateway, DNS, SSH
- **Administração Linux:** contas, grupos, permissões, systemd e journalctl
- **Containers:** Docker, Docker Compose, Nginx e Apache
- **Próximos estudos:** monitoramento, Ansible, Terraform e AWS (ainda não implementados neste projeto)

## Documentação

| Documento | Conteúdo |
| --- | --- |
| [Visão geral do ambiente](docs/01-ambiente.md) | Componentes, escopo e arquitetura |
| [Administração Linux e SSH](docs/02-linux-ssh.md) | Comandos explicados e verificações |
| [Docker e Compose](docs/03-docker-compose.md) | Serviços e persistência de dados |
| [Roadmap](docs/04-roadmap.md) | Implementações atuais e planejadas |

## Como ler este projeto

Cada documento separa **atividade realizada**, **verificação** e **próximas atividades**. Comandos são acompanhados de explicações, mas devem ser adaptados ao próprio ambiente antes de executar.

## Evidências e segurança

As capturas de tela e logs serão adicionados em `evidencias/` após revisão. **Não publicar** senhas, tokens, chaves SSH privadas, arquivos `.env`, IPs corporativos, números de série nem dados de terceiros. Configurações compartilhadas devem usar valores fictícios.

## Autor

Projeto de estudos e portfólio técnico de **Matheus Silvestre de Oliveira Dias**.

---

*Documentation in Portuguese. English summary: A practical infrastructure homelab featuring Proxmox, Ubuntu Server, Linux administration, SSH, Docker Compose, and persistent volumes; future phases include monitoring, automation, and AWS.*
