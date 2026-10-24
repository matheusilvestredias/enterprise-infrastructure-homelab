# 03 — Docker, Docker Compose e persistência

## Objetivo

Executar serviços web em containers e entender o ciclo de vida de containers, redes e volumes.

## Ambiente de estudo

Foi utilizado um projeto Docker Compose com containers **Nginx** e **Apache**, além de volumes Docker para armazenamento persistente.

## Experimento de persistência

Um dos testes executados foi:

```bash
sudo docker compose down
sudo docker volume ls
```

### Explicação dos comandos

- `sudo`: executa os comandos com privilégios elevados (quando necessários).
- `docker compose down`: para e remove os containers e redes criados pelo projeto Compose. **Por padrão, não remove volumes nomeados**. A opção `-v` remove volumes declarados pelo projeto e volumes anônimos associados, exigindo cuidado.
- `docker volume ls`: lista os volumes Docker existentes.

### Resultado observado

Após o `docker compose down`, os containers e a rede do projeto foram removidos, porém volumes nomeados ainda apareciam em `docker volume ls`, demonstrando que seu ciclo de vida é independente do dos containers.

**Limite do teste:** a permanência do volume comprova que o volume não foi removido; para provar a persistência de *dados específicos*, é necessário criar um arquivo de teste, recriar o container e ler o mesmo arquivo novamente.

## Próximos testes sugeridos

1. Criar um arquivo de exemplo em um volume nomeado.
2. Registrar hash e conteúdo do arquivo.
3. Executar `docker compose down` (sem `-v`).
4. Executar `docker compose up -d`.
5. Validar se o arquivo permanece acessível.
6. Registrar prints, comandos e saídas revisados.

> Não publique segredos, senhas, configurações internas ou arquivos reais sensíveis.
