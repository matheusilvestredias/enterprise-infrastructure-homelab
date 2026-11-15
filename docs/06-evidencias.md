# 06 — Guia de evidências técnicas

## Evidências recebidas

| Código | Descrição | Situação |
| --- | --- | --- |
| E01 | Saída de `pveversion -v` no host (Proxmox VE 9.2.0; kernel 7.0.14-24-pve) | **Recebida e transcrita em 28/10/2026** |
| E02 | Saída de `qm list` (VM 100 em execução, 4096 MB RAM, 40 GB boot disk) | **Recebida e transcrita em 28/10/2026** |
| E03 | Configuração da VM via `qm config 100` | Pendente |
| E04 | Configuração de rede da VM revisada | Pendente |
| E05 | SSH ativo | Pendente imagem recente |
| E06 | Logs systemd | Pendente imagem recente |
| E07 | Containers ativos | Pendente imagem recente |
| E08 | Persistência de arquivo após recriar containers | Pendente |

> As capturas de E01/E02 foram recebidas na conversa e **ainda não estão armazenadas como arquivos de imagem no repositório público**. Os dados conferidos estão documentados em [05-proxmox-ubuntu.md](05-proxmox-ubuntu.md).

## Para publicar evidências

- Usar nomes como `evidencias/E01-proxmox-versao.png` e `evidencias/E02-qm-list.png`.
- Cortar o que não é relevante, remover IPs internos reais, MACs, seriais, credenciais, tokens e dados de terceiros.
- Evitar imagens contendo prompts com dados privados ou links de acesso remoto.
- Diferenciar teste **executado** de teste **planejado**; não marcar como concluída a validação que ainda depende de saída real.
