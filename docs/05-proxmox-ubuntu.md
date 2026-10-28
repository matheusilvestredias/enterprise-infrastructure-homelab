# 05 — Proxmox VE e VM Ubuntu Server

## Objetivo

Documentar o host de virtualização e a VM Linux utilizando resultados reais do laboratório.

## Evidência: 28/10/2026

O operador executou no host os comandos `pveversion -v` e `qm list`. As capturas foram fornecidas na conversa de documentação e os dados abaixo foram transcritos dos resultados.

### Versões confirmadas

| Componente | Versão observada |
| --- | --- |
| Proxmox VE | **9.2.0** |
| pve-manager | **9.2.21** |
| Kernel em execução | **7.0.14-24-pve** |
| QEMU/KVM (pve-qemu-kvm) | **11.0.3-4** |
| pve-container | **6.1.14** |
| pve-firewall | **6.0.6** |

O relatório completo de `pveversion -v` foi capturado, mas esta página apresenta somente os componentes centrais para facilitar a leitura.

### Máquina virtual confirmada

| Campo | Valor observado |
| --- | --- |
| VMID | `100` |
| Nome | `ubuntu-server-01` |
| Estado | `running` |
| Memória configurada | `4096 MB` (4 GiB apresentados pelo Proxmox como 4096 MB) |
| Disco de boot | `40.00 GB` |
| PID QEMU | Presente enquanto VM em execução; não mantido por ser temporário |

**Importante:** `qm list` mostra a memória configurada para a VM, **não** o uso efetivo de RAM dentro do Linux; também não informa número de vCPUs. A capacidade e o uso do disco precisam de outros comandos.

### Comandos executados

```bash
pveversion -v
qm list
```

- `pveversion -v`: lista a versão do Proxmox VE, a versão do kernel e os pacotes instalados.
- `qm list`: lista VMs QEMU/KVM, seus IDs, nomes, estados, memória configurada, disco de boot e PID.

## Topologia lógica verificada/parcial

```text
Servidor físico
  |
  +-- Proxmox VE 9.2.0 (kernel 7.0.14-24-pve)
        |
        +-- VM 100: ubuntu-server-01 [running]
              |-- RAM configurada: 4096 MB
              |-- Disco de boot: 40 GB
              |-- Ubuntu Server / SSH (registrado em testes anteriores)
              +-- Docker e serviços de laboratório (registrados em testes anteriores)
```

A bridge de rede, a quantidade de vCPUs e a configuração da VM **ainda não foram verificadas** nestas capturas.

## Outros dados históricos a confirmar

Registros anteriores do laboratório indicavam servidor com dois processadores, 128 GB de RAM física, aproximadamente 1,5 TB de armazenamento e interface `ens18` na VM. Estes itens **não foram validados** pelas capturas de 28/10/2026.

## Próximas verificações de somente leitura

No host Proxmox:

```bash
qm config 100
pvesm status
```

- `qm config 100`: mostra a configuração da VM de ID 100, incluindo CPU, memória, discos e interfaces virtuais. Atenção: revise/remova quaisquer valores sensíveis antes de publicar.
- `pvesm status`: exibe armazenamentos configurados e informações de capacidade/uso.

Dentro da VM Ubuntu:

```bash
hostnamectl
ip -br addr
systemctl status ssh --no-pager
```

- `hostnamectl`: apresenta nome do sistema e informações de SO/kernel.
- `ip -br addr`: resume interfaces e endereços IP. Oculte IPs reais antes de publicar.
- `systemctl status ssh --no-pager`: consulta a situação do serviço SSH sem abrir paginador.

## Cuidados de publicação

Não disponibilizar publicamente senhas, tokens, chaves privadas, IPs de acesso, dados da empresa ou seriais. As imagens originais podem permanecer em arquivo privado; este documento registra fatos técnicos já revisados.
