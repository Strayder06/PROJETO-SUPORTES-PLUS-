# API Suportes Plus (Service Desk / Help Desk)

Base URL (local): `http://localhost:3000/api`
Formato de dados: **JSON** para entrada e saída.

---

## Índice

* Autenticação
* Users (Usuários/Agentes)
* Customers (Clientes)
* Tickets (Chamados)
* Ticket Messages (Mensagens)
* Tags (Etiquetas)
* SLA (Políticas)
* Knowledge Articles
* Exemplos curl / Postman
* Códigos HTTP
* Boas práticas

---

## Autenticação (opcional)

### POST /auth/login

**Request Body**

```json
{
  "email": "admin@suportesplus.com",
  "password": "senha123"
}
```

**Response 200**

```json
{
  "token": "eyJhbGciOi...",
  "user": {
    "id": 1,
    "name": "Admin",
    "email": "admin@suportesplus.com",
    "role": "admin"
  }
}
```

---

## Users (Usuários)

### GET /users

Retorna todos os usuários.

### GET /users/:id

Retorna usuário específico.

### POST /users

Cria um novo usuário.

```json
{
  "name": "Maria Lima",
  "email": "maria@empresa.com",
  "phone": "11988887777",
  "role": "agent"
}
```

### PUT /users/:id

Atualiza dados do usuário.

### DELETE /users/:id

Remove usuário.

---

## Customers (Clientes)

### GET /customers

Retorna lista de clientes.

### GET /customers/:id

Retorna cliente por ID.

### POST /customers

Cria cliente.

```json
{
  "company_name": "Inova Solutions",
  "contact_name": "Ana Costa",
  "contact_email": "ana@inova.com",
  "contact_phone": "11999993333",
  "address": "Av. Central, 100"
}
```

### PUT /customers/:id

Atualiza cliente.

### DELETE /customers/:id

Exclui cliente.

---

## Tickets (Chamados)

### GET /tickets

Suporta filtros: `status`, `priority`, `customer_id`, `assigned_id`, `page`, `limit`.

### GET /tickets/:id

Retorna ticket com detalhes e relacionamentos.

```json
{
  "id": 45,
  "external_id": "SUP-1001",
  "title": "Erro no login",
  "description": "Usuário vê tela em branco.",
  "status": "open",
  "priority": "high",
  "customer": {"id": 3, "company_name": "Tech"},
  "requester": {"id": 5, "name": "Carlos"},
  "assigned": {"id": 2, "name": "Agente X"},
  "messages": [],
  "tags": []
}
```

### POST /tickets

Cria ticket.

```json
{
  "external_id": "SUP-1002",
  "title": "Sistema lento",
  "description": "Linaisão nas consultas",
  "status": "open",
  "priority": "medium",
  "customer_id": 3,
  "requester_id": 5
}
```

### PUT /tickets/:id

Atualiza dados do ticket.

### DELETE /tickets/:id

Remove ticket.

---

## Ticket Messages

### GET /tickets/:ticket_id/messages

Lista mensagens do ticket.

### POST /tickets/:ticket_id/messages

Cria nova mensagem.

```json
{
  "author_user_id": 2,
  "author_customer_id": null,
  "body": "Estamos verificando."
}
```

---

## Tags

### GET /tags

Lista tags.

### POST /tags

Cria tag.

### POST /tickets/:ticket_id/tags/:tag_id

Associa tag ao ticket.

### DELETE /tickets/:ticket_id/tags/:tag_id

Remove associação.

---

## SLA (Políticas)

### GET /sla

Lista políticas SLA.

### POST /sla

Cria política.

```json
{
  "name": "Padrão",
  "response_time_minutes": 30,
  "resolution_time_minutes": 240
}
```

### POST /customers/:id/sla

Associa SLA a cliente.

---

## Knowledge Articles (Base de Conhecimento)

### GET /articles

Lista artigos.

### GET /articles/:id

Retorna artigo.

### POST /articles

Cria artigo.

### PUT /articles/:id

Atualiza artigo.

### DELETE /articles/:id

Remove artigo.

---

## Exemplos curl

### Criar cliente

```bash
curl -X POST http://localhost:3000/api/customers \
  -H "Content-Type: application/json" \
  -d '{"company_name":"Nova Ltda","contact_name":"Pedro"}'
```

### Criar ticket

```bash
curl -X POST http://localhost:3000/api/tickets \
  -H "Content-Type: application/json" \
  -d '{"external_id":"SUP-2001","title":"Erro"}'
```

---

## Códigos HTTP

* 200 OK
* 201 Created
* 400 Bad Request
* 401 Unauthorized
* 403 Forbidden
* 404 Not Found
* 500 Internal Server Error

Erro recomendado:

```json
{ "error": "Mensagem clara", "details": {} }
```

---

## Boas práticas

* Usar variáveis de ambiente (.env).
* Validar dados de entrada.
* Implementar paginação.
* Manter padronização de nomes.
* Documentação sempre atualizada.
