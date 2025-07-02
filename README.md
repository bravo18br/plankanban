# 📌 Kanban Interno com Planka via Docker

Este projeto disponibiliza um sistema Kanban estilo Trello, utilizando o [Planka](https://github.com/plankanban/planka), rodando via Docker e conectado a um banco PostgreSQL com suporte a vetores (`pgvector`).

---

## 🚀 Pré-requisitos

- Docker e Docker Compose instalados
- Banco de dados `pgvector` já em execução na porta interna `5432` e na rede `pgvector_net`

---

## ⚙️ Preparação do Banco de Dados

Antes de subir o container do Planka, **crie o banco e o usuário** no PostgreSQL:

```sql
CREATE USER planka WITH PASSWORD 'planka';
CREATE DATABASE planka OWNER planka;
````

> Dica: você pode salvar esse script como `initdb/create_planka.sql` e montar como volume em `/docker-entrypoint-initdb.d` caso queira automatizar a criação.

---

## 📦 Subindo os containers

Use o seguinte comando para iniciar o Planka:

```bash
docker compose up -d
```

---

## 🌐 Acesso ao sistema

Abra seu navegador e acesse:

```
http://localhost:1337
```

---

## 🔐 Primeiro acesso (credenciais padrão)

Planka já vem com um usuário administrador criado por padrão:

* **Usuário**: `admin`
* **Senha**: `admin`

> ⚠️ **Importante**: Altere a senha após o primeiro login!

---

## 🧩 Variáveis principais usadas

No serviço `planka`, as variáveis mais importantes são:

```env
DATABASE_URL=postgres://planka:planka@pgvector:5432/planka
SECRET_KEY=uma_senha_bem_forte
TRUST_PROXY=1
```

---

## 🩺 Healthcheck

Um `healthcheck` HTTP verifica se o serviço do Planka está acessível:

```yaml
healthcheck:
  test: ["CMD", "wget", "--spider", "-q", "http://localhost:1337"]
  interval: 30s
  timeout: 10s
  retries: 3
  start_period: 10s
```

---

## 🧰 Rede Docker

O serviço usa uma rede externa chamada `pgvector_net`. Certifique-se de que ela existe:

```bash
docker network create pgvector_net
```

Ou, se já criada por outro serviço, apenas garanta que ambos os containers estejam nela.

---

## 📄 Licença

Este projeto segue a licença [MIT](https://opensource.org/licenses/MIT). O Planka é um software open source mantido pela comunidade.

---
