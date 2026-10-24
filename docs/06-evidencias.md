# 06 — Guia de evidências técnicas

A documentação deve mostrar **o que foi feito**, **como foi testado** e **qual foi o resultado**.

## Evidências propostas

| Evidência | Conteúdo esperado | Situação |
| --- | --- | --- |
| E01 | Tela de VMs no Proxmox | Pendente |
| E02 | Versão do host Proxmox | Pendente |
| E03 | Configuração de hardware da VM Ubuntu | Pendente |
| E04 | IP/rotas/DNS anonimizados da VM | Pendente |
| E05 | SSH ativo | Pendente |
| E06 | Logs do serviço systemd | Pendente |
| E07 | Containers Nginx e Apache ativos | Pendente |
| E08 | Teste de persistência real de arquivo após recriação dos containers | Pendente |

## Padrão de nome

`evidencias/E01-proxmox-vms.png`, `evidencias/E05-ssh-ativo.png`, etc.

## Regras de publicação

- Não incluir credenciais, tokens, segredos, chaves privadas, dados de clientes ou da empresa.
- Ocultar IPs internos reais, MACs, seriais, nomes de host organizacionais e nomes de usuários privados.
- Guardar as capturas originais fora do repositório público; publicar apenas cópias revisadas.
- Não considerar um teste comprovado apenas por um comando anotado: anexar saída revisada ou screenshot.
