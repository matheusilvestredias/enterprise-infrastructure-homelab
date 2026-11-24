# Enterprise Infrastructure Homelab

> Laboratório prático de Infraestrutura de TI, Linux, virtualização, redes e containers, com evolução planejada para observabilidade, automação e Cloud.

## Visão geral

Este repositório documenta a construção progressiva de um ambiente de estudo com servidor físico, **Proxmox VE**, **Ubuntu Server** e **Docker**. O objetivo é demonstrar competências práticas de administração de sistemas, troubleshooting e documentação técnica.

**Status:** em desenvolvimento. **Última atualização (24/11/2026):** automação Ansible com templates Jinja2 validada, bridge isolada criada e instalação do OPNsense iniciada para segmentação de rede. A validação de roteamento, firewall e DNS entre segmentos permanece pendente.

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

    └── VM103 OPNsense (em implantação)
        ├── WAN: vmbr0 (VLAN-aware)
        └── LAN: vmbr1 (bridge isolada)

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
- **Segmentação e segurança:** OPNsense, bridges Linux, VLAN-aware, interfaces WAN/LAN
- **Templates:** Jinja2 e idempotência Ansible
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

## Evolução — novembro de 2026

### Automação de configuração (19–20/11)

- Uso de templates Jinja2 com Ansible para gerar uma página dinâmica no Nginx da VM101.
- Página servida com informações do sistema Ubuntu Server 24.04 e memória.
- Reexecução do playbook com `changed=0`, confirmando idempotência nessa execução.

### Segmentação de rede e firewall (24/11)

- Bridge `vmbr0` confirmada como VLAN-aware (`vlan_filtering 1`).
- Bridge `vmbr1` criada e ativa, sem interface física associada.
- VM103 criada para OPNsense, com WAN em `vmbr0` e LAN em `vmbr1`.
- Instalação UFS em disco virtual de 32 GB, com configuração de swap de 8 GB registrada.

**Próximas validações:** concluir configuração das interfaces, testar conectividade entre segmentos, regras de firewall, roteamento, NAT e resolução DNS. Não há confirmação de funcionamento integral desses recursos nesta etapa.

## Como ler este projeto

Cada documento separa **atividade realizada**, **verificação** e **próximas atividades**. Comandos são acompanhados de explicações, mas devem ser adaptados ao próprio ambiente antes de executar.

## Evidências e segurança

As capturas de tela e logs serão adicionados em `evidencias/` após revisão. **Não publicar** senhas, tokens, chaves SSH privadas, arquivos `.env`, IPs corporativos, números de série nem dados de terceiros. Configurações compartilhadas devem usar valores fictícios.

## Autor

Projeto de estudos e portfólio técnico de **Matheus Silvestre de Oliveira Dias**.

---

*Documentation in Portuguese. English summary: A practical infrastructure homelab featuring Proxmox, Ubuntu Server, Linux administration, SSH, Docker Compose, Flask, PostgreSQL and persistent volumes; Ansible automation across two Ubuntu VMs; future phases include monitoring, Terraform and AWS.*
