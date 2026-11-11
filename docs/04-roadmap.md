# 04 — Roadmap do laboratório

## Implementado / praticado

- [x] Servidor de virtualização Proxmox VE
- [x] VM Ubuntu Server
- [x] Diagnóstico de rede, gateway e DNS
- [x] Administração Linux e SSH
- [x] Serviço systemd e consulta de logs
- [x] Docker e Docker Compose
- [x] Aplicação multicontainer com Nginx, Flask e PostgreSQL; containers em execução e endpoint `/equipamentos` respondendo (11/11/2026)
- [x] Observação de volumes persistindo após remoção dos containers

## Próximas etapas (não concluídas)

- [ ] Revisar e publicar arquivos Compose, Nginx e Flask sem segredos
- [ ] Validar healthchecks e ordem de dependências dos serviços
- [ ] Comprovar persistência do PostgreSQL e procedimento de backup/restauração
- [ ] Ampliar testes e evidências do Docker Compose
- [ ] Monitoramento de hosts e serviços (Zabbix/Prometheus/Grafana — escolha pendente)
- [ ] Automação de configurações com Ansible
- [ ] Infraestrutura como código com Terraform
- [ ] Integração com AWS, incluindo custos, permissões e segurança
- [ ] Diagramas revisados e portfólio de publicações no LinkedIn

## Critério de conclusão de cada etapa

Uma etapa só será marcada como validada quando houver descrição de ambiente, procedimento, comandos explicados, resultado de teste e evidências sanitizadas.
