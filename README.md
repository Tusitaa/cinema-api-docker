# Cinema API CP2

API REST para gerenciamento de filmes e ingressos, desenvolvida com Java, Spring Boot, Spring Data JPA, MySQL e Swagger/OpenAPI.

## Requisitos

- Java 17
- Docker
- Maven Wrapper ja incluido no projeto:
  - Windows: `mvnw.cmd`
  - Linux/macOS: `./mvnw`

## Como subir o banco de dados com Docker

A aplicacao esta configurada para acessar o MySQL em `localhost:3306`, usando o banco `cinema`, usuario `root` e senha `root_pwd`.

Execute o comando abaixo na raiz do projeto:

```bash
docker run -d \
    --name mysql \
    --rm \
    -e MYSQL_ROOT_PASSWORD=root_pwd \
    -e MYSQL_USER=new_user \
    -e MYSQL_PASSWORD=my_pwd \
    -p 3306:3306 \
    mysql
```

Para verificar se o container esta em execucao:

```bash
docker ps
```

## Como rodar a aplicacao

Com o banco de dados em execucao, rode a API pela raiz do projeto.

No Windows:

```bash
mvn spring-boot:run
```

No Linux/macOS:

```bash
./mvnw spring-boot:run
```

A API ficara disponivel em:

```text
http://localhost:8080
```

## Swagger/OpenAPI

O Swagger esta configurado na raiz da aplicacao:

```text
http://localhost:8080/
```

Use essa pagina para visualizar e testar as rotas da API.

## Rotas principais

Versao da API configurada: `v1`.

### Filmes

- `POST /api/v1/filmes`
- `GET /api/v1/filmes`
- `GET /api/v1/filmes/{id}`
- `PUT /api/v1/filmes/{id}`
- `DELETE /api/v1/filmes/{id}`

Exemplo de JSON para criar ou atualizar um filme:

```json
{
  "id": 1,
  "titulo": "Interestelar",
  "genero": "Ficcao cientifica",
  "duracaoMinutos": 169,
  "diretor": "Christopher Nolan"
}
```

### Ingressos

- `POST /api/v1/ingressos`
- `GET /api/v1/ingressos`
- `GET /api/v1/ingressos/{id}`
- `PUT /api/v1/ingressos/{id}`
- `DELETE /api/v1/ingressos/{id}`

Exemplo de JSON para criar ou atualizar um ingresso:

```json
{
  "id": 1,
  "sessao": "20:30",
  "quantidadeDisponivel": 120,
  "preco": 35.5,
  "sala": "Sala 1"
}
```

## Como rodar a API com Docker

Na raiz do projeto, crie a imagem:

```bash
docker build -t cinema-api:1.0 .
```

Com o MySQL rodando na maquina host, use `host.docker.internal` para que o
container consiga acessar o banco:

```bash
docker run --rm \
    -p 8080:8080 \
    -e DB_SERVER_URL=host.docker.internal \
    -e DB_SERVER_PORT=3306 \
    -e DB_SCHEMA=cinema \
    -e DB_USER=root \
    -e DB_PWD=root_pwd \
    cinema-api:1.0
```

A API fica disponivel em `http://localhost:8080`.

## Configuracao do banco

As configuracoes estao em `src/main/resources/application.properties` e podem ser
sobrescritas por variaveis de ambiente:

| Variavel | Descricao | Padrao |
|---|---|---|
| `DB_SERVER_URL` | Endereco do servidor MySQL | `localhost` |
| `DB_SERVER_PORT` | Porta do MySQL | `3306` |
| `DB_SCHEMA` | Nome do banco | `cinema` |
| `DB_USER` | Usuario do banco | `root` |
| `DB_PWD` | Senha do banco | `root_pwd` |
| `SPRING_PROFILES_ACTIVE` | Profile ativo do Spring | `dev` no container |

Sem nenhuma variavel definida, a aplicacao usa `localhost:3306` e o banco `cinema`,
o que atende a execucao local com `mvn spring-boot:run`.

O profile `dev` (padrao na imagem Docker) usa o banco `cinema_dev` para nao
misturar os dados com a execucao local.

O Hibernate esta configurado com `ddl-auto=update`, entao as tabelas sao criadas ou atualizadas automaticamente ao iniciar a aplicacao.
