# 02 — Ubuntu Server, administração Linux e SSH

## Escopo praticado

Configuração de interfaces de rede, diagnóstico de resolução DNS, administração de usuários e grupos, permissões, acesso SSH e serviços systemd.

## Comandos utilizados e sua função

| Comando | Para que serve |
| --- | --- |
| `ip addr` | Lista interfaces e endereços IP atribuídos. |
| `resolvectl status` | Mostra a configuração atual de DNS por interface. |
| `resolvectl query exemplo.com` | Consulta a resolução DNS do nome informado. |
| `ss -tulpn` | Lista sockets TCP/UDP em escuta e processos associados (conforme permissões). |
| `sudo systemctl status ssh` | Consulta o estado do serviço SSH. |
| `id usuario` | Mostra UID, GID e grupos de determinado usuário. |
| `namei -l /caminho/arquivo` | Detalha permissões dos componentes do caminho. |
| `chmod` | Modifica permissões de arquivo ou diretório. |
| `chown` | Altera proprietário e/ou grupo de arquivo ou diretório. |
| `sudo systemctl status laboratorio.service` | Confere estado do serviço personalizado de laboratório. |
| `journalctl -u laboratorio.service` | Exibe logs registrados para a unidade systemd. |

> **Atenção:** `chmod` e `chown` exigem argumentos específicos. Não aplique alterações amplas/recursivas sem verificar o caminho e o impacto.

## Verificações realizadas no laboratório

- Interface de rede da VM e conectividade IPv4 verificadas.
- Resolução DNS inspecionada.
- Serviço SSH verificado como ativo.
- Permissões de usuários e grupos testadas.
- Serviço personalizado systemd criado e logs acompanhados.

## Próxima documentação

Adicionar os arquivos de configuração utilizados (com dados sensíveis removidos), screenshots revisados e resultados reproduzíveis de testes.
