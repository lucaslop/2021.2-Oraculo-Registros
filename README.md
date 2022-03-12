# 2021.1-Oraculo-Registros

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/gpl-3.0.html)

[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=fga-eps-mds_2021.1-Oraculo-Processos&metric=alert_status)](https://sonarcloud.io/dashboard?id=fga-eps-mds_2021.1-Oraculo-Registros)

## Como contribuir?

Gostaria de contribuir com nosso projeto? Acesse o nosso [guia de contribuição](https://fga-eps-mds.github.io/2021.1-Oraculo/CONTRIBUTING/) onde são explicados todos os passos.
Caso reste duvidas você também pode entrar em contato conosco criando uma issue.

## Documentação

A documentação do projeto pode ser acessada pelo nosso site em https://fga-eps-mds.github.io/2021.1-Oraculo/.

## Testes

Todas as funções adicionadas nessa API devem ser testadas, o repositŕorio aceita até 10% do total de linhas não testadas. Para rodar os testes nesse repositŕio deve ser executado o comando:
p
yarn run db:migrate
yarn run test
```

**Importante**: o padrão da variável DATABASE_URL é: `postgres://oraculo:oraculo123@localhost:5431/oraculo`

## Como rodar?

O arquivo `.env` possui configurações iniciais que podem ser alteradas de acordo com a necessidade. São elas:

- SECRET: chave para criptografia das senhas
- DB_USER: usuário de acesso ao banco de dados
- DB_PASS: senha de acesso ao banco de dados
- DB_NAME: nome da base de dados
- DB_HOST: host da base de dados
- DB_PORT: porta de conexão com o banco

Veja o exemplo abaixo:

```
SECRET=chavedesegredo
DB_USER=api_user
DB_PASS=api_password
DB_NAME=api_database
DB_HOST=db_users
```

Para rodar a API é preciso usar os seguintes comandos usando o docker:

1 - Instale as dependências

```bash
yarn
```

1.1 - Certifique-se de limpar containers já existentes

```bash
yarn docker:clean
```

2 - Configure as variáveis de ambiente editando o arquivo `.env`

```
SECRET=chavedesegredo
DB_USER=api_user
DB_PASS=api_password
DB_NAME=api_database
DB_HOST=db_users
DB_PORT=8001
```

3 - Configure a variável de ambiente `DATABASE_URL`

```bash
export DATABASE_URL=postgres://${DB_USER}:${DB_PASS}@${DB_HOST}:${DB_PORT}/${DB_NAME}
```

3.1 (não obrigatório) - Configure o CORS

```bash
export CORS=http://localhost:3000
```

**Importante**: os valores das variáveis DB_USER, DB_PASS, DB_HOST, DB_PORT e DB_NAME são os mesmos
do arquivo `.env` editado anteriormente.

Se o arquivo `.env` estiver com os mesmos valores do passo anterior, então a `DATABASE_URL` deverá ser exportada
da seguinte forma:

```bash
export DATABASE_URL=postgres://api_user:api_password@db_users:8001/api_database
```

4 - Suba o container

```bash
yarn all:prod
```

A API estará rodando na porta especificada pela variável `DB_PORT` (padrão é a porta 8001)

## Rotas

**GET: `/records`**

Endpoint para exibir todos os registros

- Resposta

```json
[
  {
    "id": 2,
    "register_number": "",
    "inclusion_date": "",
    "city": "",
    "state": "",
    "requester": "",
    "document_type": "",
    "document_number": "",
    "document_date": "",
    "description": "",
    "sei_number": "",
    "receipt_form": "",
    "contact_info": "",
    "situation": "",
    "created_by": "",
    "assigned_to": "",
    "updatedAt": "",
    "createdAt": ""
  },
  {
    "id": 2,
    "register_number": "",
    "inclusion_date": "",
    "city": "",
    "state": "",
    "requester": "",
    "document_type": "",
    "document_number": "",
    "document_date": "",
    "description": "",
    "sei_number": "",
    "receipt_form": "",
    "contact_info": "",
    "situation": "",
    "created_by": "",
    "assigned_to": "",
    "updatedAt": "",
    "createdAt": ""
  }
]
```

- **assigned_to** é o id do usuário atualmente responsável por aquele registro

**GET: `/records/:id`**

Retorna os dados de um registro específico, só precisando do id no link

- Resposta

```json
{
  "id": 1,
  "register_number": "",
  "inclusion_date": "",
  "city": "",
  "state": "",
  "requester": "",
  "document_type": "",
  "document_number": "",
  "document_date": "",
  "description": "",
  "sei_number": "",
  "receipt_form": "",
  "contact_info": "",
  "situation": "",
  "created_by": "",
  "assigned_to": "",
  "updatedAt": "",
  "createdAt": ""
}
```

**POST: `/records`**

Para criar um registro, envie os dados nesse formato:

- Requisição

```json
{
  "city": "",
  "state": "",
  "requester": "",
  "document_type": "",
  "document_number": "",
  "document_date": "",
  "description": "",
  "sei_number": "",
  "receipt_form": "",
  "contact_info": "",
  "created_by": ""
}
```

- **situation** poderá ser: `finished` (0), `running` (1) ou `pending` (2)

- Resposta

```json
{
  "id": 2,
  "register_number": "",
  "inclusion_date": "",
  "city": "",
  "state": "",
  "requester": "",
  "document_type": "",
  "document_number": "",
  "document_date": "",
  "description": "",
  "sei_number": "",
  "receipt_form": "",
  "contact_info": "",
  "created_by": "",
  "assigned_to": "",
  "updatedAt": "",
  "createdAt": "",
  "situation": "",
  "tags": [],
  "departments": [
    {
      "id": 2,
      "name": "",
      "is_admin": false,
      "createdAt": "2021-11-06T00:56:10.590Z",
      "updatedAt": "2021-11-06T00:56:10.590Z",
      "record_departments": {
        "createdAt": "2021-11-06T01:11:55.798Z",
        "updatedAt": "2021-11-06T01:11:55.798Z",
        "record_id": 2,
        "department_id": 0
      }
    }
  ]
}
```

**POST: `/records/:id/forward`**

Para encaminhar um registro, basta envie os dados nesse formato:

```json
{
  "destination_id": 0,
  "origin_id": 0,
  "forwarded_by": ""
}
```

Resposta esperada:

```json
{
  "forwarded_by": "",
  "forwarded_by_name": "",
  "forwarded_from": "",
  "forwarded_to": ""
}
```

- **id** é o id do registro a ser encaminhado
- **destination_id** é o id da seção de destino
- **forwarded_by_name** é o nome do usuário que encaminhou o registro
- **forwarded_by** é o email do usuário que encaminhou o registro. Na resposta esse campo será uma string que contém o nome do usuário que encaminhou o registro
- **origin_id** é o id da seção de origem (é o id da seção ao qual o usuário que encaminhou pertence)

**GET: `/records/:id/departments`**

Para obter o histórico de seções por onde um registro passou, envie uma request
contendo o **id** do registro do qual você quer obter o histórico de seções

**GET: `/records/page/:page`**

Caso queira retornar os registros de forma paginada, envie
uma request no formato acima, aonde:

- :page: é o número da página que você deseja acessar. Esse número
  deverá ser incrementado de acordo com o limite de registros por página.

Exemplo:

- **GET: `/records/page/0`** irá retornar os 4 primeiros registros
- **GET: `/records/page/4`** irá retornar os 4 registros seguintes

**POST: `/records/:id/situation`**

Caso queira atualizar o status de um processo, envie os dados no formato a seguir:

```json
{
  "situation": ""
}
```

**GET: `/records/fields`**

Retorna dados sobre os campos dos registros

Resposta:

```json
{
  "name": "",
  "description": "",
  "created_by": ""
}
```

Se o `created_by` for vazio, então significa que ele é um campo "default"

**GET: `/records/:id/history`**

Retorna todo o histórico de movimentação de um registro

```json
{
  "id": 0,
  "forwarded_by": "",
  "origin_id": 0,
  "origin_name": "",
  "destination_id": 0,
  "destination_name": "",
  "closed_by": "",
  "closed_at": "",
  "created_by": "",
  "created_at": "",
  "reopened_by": "",
  "reopened_at": "",
  "record_id": 0,
  "created_at": "",
  "updated_at": ""
}
```

**Importante**: alguns campos poderão ser nulos, de acordo com a informação contida na entrada do histórico.

- `created_by` será não nulo caso seja uma entada no histórico que contém informações sobre a data de criação
- `forwarded_by` será não nulo caso seja uma entrada no histórico que contém informações sobre encaminhamento de um registro
- `closed_by` será não nulo caso seja uma entrada no histórico que contém informações sobre fechamento de um registro
- `reopened_by` será não nulo caso seja uma entrada no histórico que contém informações sobre a reabertura de um registro

**GET: `/records/:id/current-department`**

Retorna a localização da seção atual

```json
{
  "current_department": 0
}
```

- **current_department** é o id da seção em que o registro está localizado no momento

**POST: `/users`**

Registra um novo usuário

```json
{
  "name": "",
  "email": "",
  "department_id": ""
}
```

**GET `/count/records`**

Irá retornar a quantidade total de registros no banco

Formato da resposta:

```json
{
  "count": 0
}
```

**GET `/records/department/:id`**

Retorna todos os registros de um departamento específico

```json
[
  {
    "id": 2,
    "register_number": "",
    "inclusion_date": "",
    "city": "",
    "state": "",
    "requester": "",
    "document_type": "",
    "document_number": "",
    "document_date": "",
    "description": "",
    "sei_number": "",
    "receipt_form": "",
    "contact_info": "",
    "situation": "",
    "created_by": "",
    "assigned_to": "",
    "updatedAt": "",
    "createdAt": ""
  },
  {
    "id": 15,
    "register_number": "",
    "inclusion_date": "",
    "city": "",
    "state": "",
    "requester": "",
    "document_type": "",
    "document_number": "",
    "document_date": "",
    "description": "",
    "sei_number": "",
    "receipt_form": "",
    "contact_info": "",
    "situation": "",
    "created_by": "",
    "assigned_to": "",
    "updatedAt": "",
    "createdAt": ""
  }
]
```

**id** é o id do departamento de onde você deseja extrair todos os registros

**GET `/records/:id/tags`**

Obtém a lista de tags associadas ao registro em questão

- **id** é o id do registro que você deseja obter as tags associadas

Resposta esperada:

- HTTP 200

```json
[
  {
    "id": 1,
    "name": "Urgente",
    "color": "#ff0000",
    "createdAt": "2021-10-30T01:01:46.995Z",
    "updatedAt": "2021-10-30T01:01:46.995Z",
    "records_tags": {
      "createdAt": "2021-10-30T01:08:04.016Z",
      "updatedAt": "2021-10-30T01:08:04.016Z",
      "record_id": 1,
      "tag_id": 1
    }
  },
    {
  {
    "id": 2,
    "name": "Tramitar",
    "color": "#0000aa",
    "createdAt": "2021-10-30T01:01:46.995Z",
    "updatedAt": "2021-10-30T01:01:46.995Z",
    "records_tags": {
      "createdAt": "2021-10-30T01:09:16.931Z",
      "updatedAt": "2021-10-30T01:09:16.931Z",
      "record_id": 1,
      "tag_id": 2
    }
  }
  },
]
```

- HTTP 204 - nenhuma tag está associada ao registro em questão
- HTTP 404 - o registro não foi encontrado
- HTTP 500 - erro interno no servidor (ver logs com `docker logs -f oraculo-registros`)

**POST `/records/:id/add-tag`**

Adiciona uma tag a um registro

- Body

```json
{
  "tag_id": 0
}
```

- **tag_id** é o id da tag a ser associada

**GET `/tags/all`**

Obtém uma lista de todas as tags cadastradas

- Resposta

```json
[
  {
    "id": 1,
    "name": "Urgente",
    "color": "#ff0000"
  },
  {
    "id": 2,
    "name": "Tramitar",
    "color": "#0000aa"
  },
  {
    "id": 3,
    "name": "Mudar",
    "color": "#faff00"
  },
  {
    "id": 500,
    "name": "Tag Qualquer",
    "color": "#ffffff"
  }
]
```

**POST `/tag/:id/edit`**

Edita a tag especificada

- Body

```json
{
  "name": "",
  "color": ""
}
```

- **name** é o nome novo da tag
- **color** é a cor nova da tag (em hexadecimal)

**POST `/tag/new`**

Cria uma nova tag

- Body

```json
{
  "name": "",
  "color": ""
}
```

- **name** é o nome da nova tag
- **color** é a cor da nova tag (em hexadecimal)

**GET `/departments`**

Retorna a lista de seções disponíveis

**POST `/records/:id/edit`**

Edita um registro cadastrado no sistema

- Campos editáveis:

```json
{
  "inclusion_date": "",
  "city": "",
  "state": "",
  "requester": "",
  "document_type": "",
  "document_number": "",
  "document_date": "",
  "description": "",
  "sei_number": "",
  "receipt_form": "",
  "contact_info": ""
}
```

- Resposta

* HTTP `200` contendo todas as informações do registro (atualizadas)

```json
{
  "inclusion_date": "",
  "city": "",
  "state": "",
  "requester": "",
  "document_type": "",
  "document_number": "",
  "document_date": "",
  "description": "",
  "sei_number": "",
  "receipt_form": "",
  "contact_info": ""
}
```

**POST `/user/by-mail`**

Busca as informações de um usuário pelo e-mail

- Requisição

```json
{
  "email": "william@pcgo.com"
}
```

- Resposta

```json
{
  "id": 1,
  "name": "william the police",
  "email": "william@pcgo.com",
  "department_id": 2,
  "createdAt": "2021-11-06T03:05:20.572Z",
  "updatedAt": "2021-11-06T03:05:20.572Z"
}
```

**POST `/records/:id/close`**

Finaliza as tramitações de um registro

- Body

```json
{
  "closed_by": "",
  "reason": ""
}
```

- Resposta

* HTTP 200 se o registro for fechado com sucesso
* HTTP 400 se o registro já estiver fechado ou se reason é vazio

**POST `/records/:id/reopen`**

Reabre as tramitações de um registro

```json
{
  "reopened_by": "",
  "reason": ""
}
```

**POST `/records/with-sei`**

Busca um registro de acordo com o número do SEI

- Body

```json
{
  "sei_number": ""
}
```

**sei_number** deverá conter o número do sei a ser pesquisado

**POST `/departments`**

Cria um departamento novo

```json
{
  "name": ""
}
```
