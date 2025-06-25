# Projeto Guess Game – Ambiente Dockerizado

Este projeto contém a infraestrutura completa para subir uma aplicação web com frontend em React, backend em Flask e banco de dados PostgreSQL, com balanceamento de carga e proxy reverso via NGINX.

---

## Arquitetura da Solução

### Serviços

| Serviço              | Função                                                                 |
|----------------------|------------------------------------------------------------------------|
| `db`                 | Banco de dados PostgreSQL para persistência de dados                   |
| `backend1`, `backend2` | Aplicações Flask que processam a lógica da API                        |
| `frontend`           | Container NGINX que serve a aplicação React **e** faz proxy para o backend |

---

### Volumes

- **postgres_data**: volume nomeado para persistência dos dados do banco.

---

### Redes e Comunicação

- Todos os serviços estão na **mesma rede Docker interna (default)**, o que permite que eles se comuniquem por nomes de serviço (ex: `postgres`, `backend1`).

---

### Balanceamento de Carga

O serviço `frontend` usa o NGINX como:

- **Proxy reverso** para rotear chamadas `/api/` para o backend
- **Balanceador de carga** entre dois containers do backend (`backend1` e `backend2`) configurados no `nginx.conf`

---

## Como subir as aplicações

### 1. Requisitos

- Docker
- Docker Compose v2+

### 2. Dentro do repositório `guess_game`, execute:

```bash
docker-compose up --build -d
```

### 3. Acesso:
Frontend: http://localhost:3000

---

## Como Atualizar os Serviço

### Atualizar o Backend

- Altere o código na aplicação Flask
- Rebuild apenas os containers do backend:

```bash
docker compose build backend1 backend2
docker compose up -d
```

### Atualizar o Frontend

- Altere os arquivos dentro da pasta frontend/
- Rebuild o frontend:

```bash
docker compose build frontend
docker compose up -d
```

### Atualizar a versão do PostgreSQL

- Edite o docker-compose.yaml, alterando a imagem:

```bash
db:
  image: postgres:17-alpine  # nova versão desejada
```

- Baixe a nova imagem e reinicie:

```bash
docker compose pull db
docker compose up -d
```

## Deletar os serviços

```bash
docker compose down
```

## Observações Finais

Para alterar o IP do backend, ajuste no arquivo Dockerfile.frontend, conforme exemplo abaixo

```bash
ARG REACT_APP_BACKEND_URL=http://localhost:5000
```