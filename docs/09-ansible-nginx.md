# 09 — Automação de infraestrutura com Ansible e Nginx

**Última validação registrada:** 13/11/2026  
**Status:** instalação remota do Nginx concluída e validada por HTTP.

## Objetivo

Automatizar a instalação e a inicialização do servidor web Nginx em uma segunda máquina Ubuntu Server utilizando **Ansible**, evitando a execução manual dos mesmos procedimentos no servidor de destino.

## Ambiente de laboratório

| Papel | Sistema / recurso |
| --- | --- |
| Nó de controle Ansible | `ubuntu-server-01`, Ubuntu Server |
| Nó gerenciado | `ubuntu-server-02`, Ubuntu Server |
| Software de automação | Ansible Core **2.16.3** |
| Serviço implantado | Nginx **1.24.0** (versão identificada no teste HTTP) |
| Diretório do projeto | `~/ansible-lab` |
| Inventário | `inventory.ini` |
| Playbook | `instalar-nginx.yml` |

Os IPs privados reais do laboratório foram omitidos intencionalmente.

## Arquitetura

```text
ubuntu-server-01 (nó de controle)
  Ansible Core 2.16.3
  ~/ansible-lab/
    ├── inventory.ini
    └── instalar-nginx.yml
            │
            │ SSH com autenticação por chave
            │ Ansible + elevação de privilégios (become)
            ▼
ubuntu-server-02 (nó gerenciado)
  └── Nginx 1.24.0
        └── HTTP/1.1 200 OK
```

## Etapas confirmadas

1. Ansible Core 2.16.3 instalado no nó de controle.
2. Acesso SSH por chave funcional ao nó gerenciado.
3. Conectividade Ansible verificada pelo módulo `ping`, com retorno **SUCCESS/pong**.
4. Arquivo de inventário `inventory.ini` e playbook `instalar-nginx.yml` utilizados.
5. Validação de sintaxe do playbook realizada com sucesso.
6. Execução do playbook com solicitação de senha para elevação de privilégios.
7. Instalação/inicialização do Nginx concluída com **failed=0** na execução relatada.
8. Requisição HTTP HEAD ao nó gerenciado retornou **HTTP/1.1 200 OK**, com cabeçalho `Server: nginx/1.24.0 (Ubuntu)`.

## Comandos utilizados ou verificados

```bash
ansible --version
ansible -i inventory.ini all -m ping
ansible-playbook -i inventory.ini instalar-nginx.yml --syntax-check
ansible-playbook -i inventory.ini instalar-nginx.yml --ask-become-pass
curl -I http://<IP_DO_SERVIDOR_GERENCIADO>
```

- `ansible --version`: consulta a versão da instalação Ansible.
- `ansible -i inventory.ini all -m ping`: consulta os hosts do inventário com o módulo de teste de conectividade Ansible, **não** com ping ICMP.
- `ansible-playbook -i inventory.ini instalar-nginx.yml --syntax-check`: analisa a sintaxe do playbook sem executar as tarefas.
- `ansible-playbook -i inventory.ini instalar-nginx.yml --ask-become-pass`: executa o playbook nos hosts definidos pelo inventário e solicita a senha para tarefas com `become`.
- `curl -I http://<IP_DO_SERVIDOR_GERENCIADO>`: envia requisição HTTP HEAD ao servidor de destino e exibe os cabeçalhos da resposta.

**Atenção:** os comandos acima são um roteiro representativo dos passos registrados. O inventário e o playbook **originais ainda não foram copiados para o repositório**; nenhum conteúdo YAML foi presumido.

## Evidência de resultado

Trecho sanitizado da resposta HTTP obtida em **13/11/2026**:

```http
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Content-Type: text/html
Content-Length: 615
```

O resultado confirma que o Nginx respondeu por HTTP no servidor gerenciado no momento do teste. **Não comprova**, isoladamente, HTTPS, disponibilidade externa, monitoramento nem publicação de uma página personalizada.

## Próximas etapas (pendentes)

- [ ] Revisar e publicar o inventário como exemplo com nomes/IPs fictícios.
- [ ] Revisar e publicar o conteúdo original do playbook `instalar-nginx.yml`, retirando quaisquer segredos.
- [ ] Confirmar idempotência executando novamente o playbook e observando o estado `changed`.
- [ ] Executar/validar o playbook `configurar-site.yml` para publicar uma página HTML personalizada.
- [ ] Registrar evidências sanitizadas da aplicação web e da segunda execução.
- [ ] Considerar automação de configuração de firewall e observabilidade em etapas posteriores.

## Cuidados de segurança

Não publicar credenciais, senhas, chaves SSH privadas, inventários com endereços internos reais nem dados de máquinas de terceiros.
