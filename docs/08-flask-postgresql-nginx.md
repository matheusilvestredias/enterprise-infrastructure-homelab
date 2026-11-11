# 08 — Aplicação multicontainer: Nginx, Flask e PostgreSQL

**Atualização registrada:** 11/11/2026  
**Ambiente:** VM Ubuntu Server do homelab, diretório de trabalho `~/docker-compose-lab`  
**Status:** implementação funcional observada; testes de resiliência ainda pendentes.

## Objetivo

Integrar proxy reverso, API Python e banco de dados relacional em uma única aplicação Docker Compose, simulando a separação de responsabilidades comum em ambientes corporativos.

## Arquitetura lógica

```text
Cliente HTTP autorizado
       |
       | host:8081
       v
  nginx-compose
     Nginx
       |
       | app:5000 (rede interna Compose)
       v
  flask-compose
    Flask API
       |
       | database:5432 (rede interna Compose)
       v
postgres-compose
   PostgreSQL
       |
  Volume persistente
```

Os nomes de serviço `app` e `database` representam a configuração lógica registrada na atividade; nomes efetivos, detalhes de redes e nome do volume dependem do arquivo Compose, que ainda não foi publicado aqui. O acesso a partir de VPN foi discutido, mas não é considerado validado nesta etapa.

## Evidências observadas

Foi executado no diretório do projeto:

```bash
sudo docker compose up -d
sudo docker compose ps
```

A saída do Compose indicou estes três containers com status **Running**:

| Container | Função | Publicação de porta no host |
| --- | --- | --- |
| `nginx-compose` | Proxy reverso / servidor HTTP | `8081:80/tcp` |
| `flask-compose` | API Python Flask | Sem publicação externa observada |
| `postgres-compose` | Banco PostgreSQL | Sem publicação externa observada |

Também foi registrado um teste de `curl` à rota HTTP `/equipamentos` por meio do Nginx, com resposta contendo dois registros de equipamentos de laboratório. Os endereços IP dos equipamentos não são reproduzidos neste documento público.

**Interpretação:** os containers iniciaram e o endpoint respondeu no momento do teste. Isso demonstra funcionamento básico da integração, mas não confirma testes de carga, alta disponibilidade ou backup/restauração.

## Configuração e cuidados de segurança

- Arquivo `.env` utilizado para fornecer variáveis de ambiente à aplicação e ao banco.
- `.env` incluído no `.gitignore` do projeto local, conforme registro da atividade. **Ainda não foi verificado se está fora do histórico Git remoto**.
- Somente o Nginx publica uma porta no host na listagem observada; Flask e PostgreSQL permanecem acessíveis na rede de containers.
- Nunca publicar senhas, credenciais PostgreSQL, tokens, arquivos `.env` reais ou IPs internos do laboratório.
- Um `.env.example` sem segredos é recomendável para documentar os nomes das variáveis quando o Compose for revisado.

## Comandos utilizados e explicações

```bash
sudo docker compose up -d
sudo docker compose ps
curl http://localhost:8081/equipamentos
```

- `sudo`: executa o comando com privilégios administrativos.
- `docker compose up -d`: cria/inicia os serviços definidos no projeto Compose e os deixa em segundo plano; pode recriar containers quando a configuração muda.
- `docker compose ps`: lista os containers do projeto Compose, seu estado e portas.
- `curl http://localhost:8081/equipamentos`: faz uma requisição HTTP à rota local exposta pelo Nginx; permite verificar a resposta da API por trás do proxy.

## Próximas validações — ainda não comprovadas

- [ ] Revisar `compose.yaml`, Dockerfile, configuração Nginx e código Flask antes de publicar.
- [ ] Confirmar variáveis necessárias com um `.env.example` sem valores sensíveis.
- [ ] Implantar/verificar `healthcheck` dos serviços.
- [ ] Implementar/verificar `depends_on` com dependência baseada em saúde quando adequado.
- [ ] Confirmar persistência dos dados PostgreSQL após recriar os containers.
- [ ] Testar falha e recuperação de serviços.
- [ ] Documentar backup e restauração do banco.
- [ ] Validar acesso autorizado fora da própria VM e as regras de firewall.
- [ ] Adicionar evidências sanitizadas dos testes.

## Relação com etapas anteriores

O laboratório já havia validado Nginx e Apache como containers HTTP independentes em outubro de 2026. Esta etapa é **uma evolução distinta**, adicionando API Flask, PostgreSQL e proxy reverso Nginx. Não implica que o container Apache anterior continua ativo no projeto atual.
