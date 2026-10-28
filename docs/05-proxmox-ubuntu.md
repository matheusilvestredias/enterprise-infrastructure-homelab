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

A configuração detalhada foi confirmada em 28/10/2026 pelo comando `qm config 100`:

| Parâmetro | Valor confirmado | Interpretação |
| --- | --- | --- |
| `cores: 2` | 2 | Núcleos virtuais por socket |
| `sockets: 1` | 1 | 1 socket virtual, total de **2 vCPUs** |
| `memory: 4096` | 4096 MiB | **4 GiB** de memória provisionada |
| `cpu: x86-64-v2-AES` | x86-64-v2-AES | Modelo de CPU virtual |
| `scsi0` | `local-lvm:vm-100-disk-0` | Disco virtual de 40 GiB, interface SCSI |
| `scsihw: virtio-scsi-single` | VirtIO SCSI | Controladora de armazenamento paravirtualizada |
| `iothread=1` | Habilitado | Thread dedicada de I/O para o disco |
| `net0` | VirtIO / `vmbr0` | Adaptador de rede virtual ligado à bridge do host |
| `firewall=1` | Habilitado na interface | Opção de firewall da interface ativa; **não prova existência de regras bloqueando tráfego** |
| `agent: 1` | Habilitado na configuração | Integração QEMU Guest Agent configurada; não confirma que o serviço no guest está ativo |
| `numa: 0` | Desativado | Exposição de topologia NUMA não habilitada |
| `boot` | `scsi0,ide2,net0` | Ordem de tentativa de inicialização |

Os endereços MAC, UUIDs e identificadores específicos da instância foram omitidos deliberadamente.

### Diagrama da VM confirmado

```text
Proxmox VE 9.2.0
└── vmbr0 (bridge do host)
    └── VM 100 — ubuntu-server-01 [running]
        ├── 1 socket × 2 cores = 2 vCPUs
        ├── RAM: 4096 MiB (4 GiB)
        ├── CPU: x86-64-v2-AES
        ├── VirtIO Ethernet (firewall=1 na configuração)
        ├── VirtIO SCSI / local-lvm / disco de 40 GiB
        └── QEMU Guest Agent habilitado na configuração
```

Essa topologia descreve as conexões virtuais observadas. A conexão física da bridge com interfaces do servidor ainda precisa ser verificada.

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

## Verificação da configuração virtual — 28/10/2026

Comando executado no host:

```bash
qm config 100
```

- `qm`: gerencia máquinas virtuais QEMU/KVM no Proxmox.
- `config`: consulta os parâmetros configurados.
- `100`: ID da VM consultada.

Próximas checagens sem alteração de configuração: `qm guest cmd 100 ping` para verificar se o QEMU Guest Agent responde (só funciona se instalado e operacional dentro do guest), e `ip -br link` no host para inspecionar interfaces e bridges. Evitar divulgar MACs reais das saídas.

## Validação de rede e QEMU Guest Agent — 28/10/2026

Captura fornecida pelo operador com resultados de `qm guest cmd 100 ping` e `ip -br link` no host Proxmox.

### Guest Agent

```bash
qm guest cmd 100 ping
```

O comando retornou ao prompt **sem mensagem de erro ou saída adicional**. Isso é compatível com resposta bem-sucedida ao comando `guest-ping` do QEMU Guest Agent. **Não é um teste de ping ICMP nem comprova conectividade externa.**

### Interfaces observadas (endereços MAC omitidos)

| Interface | Estado mostrado | Interpretação |
| --- | --- | --- |
| `lo` | UNKNOWN; UP/LOWER_UP | Interface loopback |
| `nic0`, `nic1`, `nic2` | DOWN | Interfaces físicas sem link ativo no momento da captura |
| `nic3` | UP/LOWER_UP | Interface física ativa |
| `vmbr0` | UP/LOWER_UP | Bridge virtual ativa |
| `tap100i0` | UNKNOWN; UP/LOWER_UP | Interface TAP da VM 100 |
| `fwbr100i0` | UP | Bridge de firewall da VM 100 |
| `fwpr100p0@fwln100i0` | UP | Porta virtual do caminho de firewall |
| `fwln100i0@fwpr100p0` | UP | Par de ligação virtual do caminho de firewall |

A presença dessas interfaces é coerente com a configuração `net0 ... bridge=vmbr0,firewall=1` da VM 100. O vínculo físico foi **confirmado posteriormente** por `bridge link`: `nic3` tem `master vmbr0` e estado `forwarding`.

### Comandos de diagnóstico utilizados

- `qm guest cmd 100 ping`: envia o comando de saúde *guest-ping* ao QEMU Guest Agent da VM 100; não testa ICMP ou Internet.
- `ip -br link`: apresenta interfaces de rede, estado operacional e atributos resumidos. Não exibe endereços IP.

### Próxima validação — somente leitura

```bash
bridge link
ip -br addr show vmbr0
```

- `bridge link`: lista portas associadas a bridges Linux, útil para identificar a interface física ligada à bridge.
- `ip -br addr show vmbr0`: mostra os endereços IP associados à bridge `vmbr0`. **Remover IPs reais antes da publicação.**

O vínculo físico e o IP na bridge foram verificados em 28/10/2026; os endereços reais permanecem omitidos na documentação pública.

## Confirmação de bridge e conectividade virtual — 28/10/2026

O comando `bridge link` confirmou as relações:

| Porta | Bridge mestre | Estado |
| --- | --- | --- |
| `nic3` | `vmbr0` | `forwarding` |
| `fwpr100p0@fwln100i0` | `vmbr0` | `forwarding` |
| `tap100i0` | `fwbr100i0` | `forwarding` |
| `fwln100i0@fwpr100p0` | `fwbr100i0` | `forwarding` |

O comando `ip -br addr show vmbr0` mostrou `vmbr0` no estado **UP** e com um endereço IPv4 **/24** atribuído, além de um endereço IPv6 link-local. Por segurança, os endereços reais e MACs não foram publicados.

### Topologia detalhada confirmada

```text
             Rede física / switch (destino a mapear)
                            |
                        nic3 [UP]
                            |
                         vmbr0 [UP]
                         /       \\
          IP do host /24          fwpr100p0
                                      |
                                  fwln100i0
                                      |
                                  fwbr100i0
                                      |
                                  tap100i0
                                      |
                           VM 100: ubuntu-server-01
```

Essa topologia representa os vínculos entre portas Linux constatados em `bridge link`; não comprova acesso ao gateway ou à Internet. O caminho de firewall indica interfaces criadas pelo Proxmox, não evidencia por si só filtragem efetiva por regras.

### Comandos e significado

- `bridge link`: exibe portas e respectivos dispositivos bridge mestre, com estado de encaminhamento.
- `ip -br addr show vmbr0`: exibe resumidamente estado e endereços IP da interface de bridge; comando não modifica a rede.

### Próximos testes

- [ ] Confirmar gateway e conectividade do host sem publicar endereços reais.
- [ ] Confirmar interface da VM, IP e gateway no Ubuntu.
- [ ] Validar comunicação do Ubuntu ao gateway, DNS e serviços da aplicação.
