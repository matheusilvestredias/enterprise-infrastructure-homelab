# 05 — Proxmox VE e VM Ubuntu Server

## Objetivo

Documentar a camada de virtualização e a instalação/gestão de uma VM Linux no laboratório. Esta documentação descreve o **ambiente trabalhado** e separa fatos observados de informações que ainda precisam de captura.

## Inventário lógico (histórico do laboratório)

| Componente | Informação documentada |
| --- | --- |
| Servidor físico | Host usado para o laboratório; histórico: dois processadores e 128 GiB de RAM instalados |
| Armazenamento físico | Aproximadamente 1,5 TB, conforme registro anterior; layout de RAID e capacidade útil exata **a confirmar** |
| Hipervisor | Proxmox VE 7.x (registros prévios mostram builds 7.0.2-6-pve e 7.0.14-24-pve; versão **atual** a confirmar) |
| VM | Ubuntu Server, identificada nos testes como `ubuntu-server-01` |
| Interface da VM | `ens18` nos registros dos testes |
| Rede | IPv4 estático /24 usado nos testes, com gateway e DNS definidos no ambiente |
| Administração | SSH, sudo, usuários/grupos, permissões e systemd |
| Aplicações | Docker e Docker Compose com Nginx e Apache |

**Segurança:** endereços do laboratório original não são reproduzidos nesta documentação pública. Os exemplos usam redes reservadas à documentação (RFC 5737), não valores de acesso real.

## Topologia lógica

```text
                     Rede local / switch
                             |
                       Host físico
                             |
                   Proxmox VE (host)
                             |
                 Bridge virtual (ex.: vmbr0)
                             |
                  VM Ubuntu Server
                      interface ens18
                             |
              +--------------+--------------+
              |              |              |
             SSH        Serviços Linux    Docker
                                           |
                                    Docker Compose
                                      /       \
                                   Nginx      Apache
```

> A bridge exibida é um **exemplo típico** de arquitetura Proxmox, não uma confirmação de que a configuração do host usa exatamente `vmbr0`. Confirmar com `ip -br link` e a configuração de rede antes de ajustar o diagrama definitivo.

## Validações realizadas durante os estudos

1. Acesso à interface web do Proxmox e gerenciamento do ambiente.
2. VM Ubuntu Server operacional.
3. Inspeção da interface de rede `ens18`, endereço IPv4 /24, gateway e DNS.
4. Acesso e estado do serviço SSH.
5. Operações com usuários, grupos e permissões.
6. Serviço personalizado `laboratorio.service` e análise de logs.
7. Containers Nginx e Apache via Docker Compose.

## Comandos para gerar evidências atuais (somente leitura)

Execute **no host Proxmox**:

```bash
pveversion -v
qm list
ip -br addr
```

- `pveversion -v`: mostra a versão instalada do Proxmox e dos pacotes relacionados.
- `qm list`: lista as máquinas virtuais QEMU/KVM e seus IDs/estados.
- `ip -br addr`: resume interfaces e IPs atribuídos ao host.

Execute **na VM Ubuntu**:

```bash
hostnamectl
ip -br addr
ip route
resolvectl status
systemctl status ssh --no-pager
```

- `hostnamectl`: mostra o hostname, kernel e informações do sistema.
- `ip -br addr`: apresenta interfaces e endereços IP de forma resumida.
- `ip route`: exibe as rotas IP, incluindo a rota padrão (gateway).
- `resolvectl status`: informa a configuração do resolvedor DNS.
- `systemctl status ssh --no-pager`: consulta o serviço SSH sem abrir o paginador interativo.

**Antes de publicar saídas/prints:** ocultar IPs reais, nomes internos, UUIDs, endereços MAC, tokens, nomes de usuários sensíveis e demais dados identificáveis.

## Checklist para transformar o registro em documentação validada

- [ ] Capturar saída **atual** de `pveversion -v` (versão exata do host).
- [ ] Confirmar modelo do servidor, memória e desenho de armazenamento/RAID divulgáveis.
- [ ] Identificar ID da VM e configuração de vCPU, RAM e disco.
- [ ] Confirmar bridge, VLANs (se houver), gateway e topologia de rede.
- [ ] Adicionar evidência da VM ligada e do SSH ativo (redigida).
- [ ] Capturar testes reproduzíveis de conectividade e DNS.
- [ ] Revisar todas as evidências antes de torná-las públicas.

## Próxima etapa

Detalhar o processo de criação da VM e registrar prints do Proxmox com dados privados ocultos. Não executar mudanças destrutivas ou migrar a infraestrutura apenas para produzir prints.
