# Testes de conectividade e containers — 28/10/2026

## Resultado observado na VM Ubuntu

| Verificação | Resultado |
| --- | --- |
| Ping ao gateway local | 4 enviados, 4 recebidos; perda de 0% |
| Latência ICMP | mínimo 0,762 ms; média 2,830 ms; máximo 8,775 ms |
| DNS para example.com | Resolução IPv4 e IPv6 bem-sucedida em 99 ms |
| Container nginx-compose | Imagem nginx; Up 4 days; host TCP 8081 para container 80 |
| Container apache-compose | Imagem httpd; Up 4 days; host TCP 8082 para container 80 |

Os endereços IP do ambiente, endereços MAC e IDs completos dos containers foram omitidos da versão pública.

## Procedimento realizado

```bash
ping -c 4 <ENDERECO_DO_GATEWAY>
resolvectl query example.com
sudo docker ps
```

- `ping -c 4`: envia quatro solicitações ICMP e calcula perda e latência de pacotes.
- `resolvectl query`: verifica a resolução de nomes por DNS.
- `sudo docker ps`: lista os containers ativos, suas imagens e portas.

## Interpretação

A resposta do gateway comprova conectividade ICMP local no instante do teste. A consulta comprova DNS funcional. A listagem do Docker confirma containers ativos e publicação das portas, mas não comprova resposta HTTP.

## Próxima validação

```bash
curl -I --max-time 10 http://127.0.0.1:8081
curl -I --max-time 10 http://127.0.0.1:8082
```

Cada `curl` solicita apenas os cabeçalhos HTTP do serviço local correspondente. `--max-time 10` impõe um limite de dez segundos.
