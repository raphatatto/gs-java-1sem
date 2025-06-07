# AquaGuard API (Java Advanced)

API REST de monitoramento de eventos extremos, parte do desafio “Global Solution 2025/1” da FIAP.

## 🔗 Links

- Repositório GitHub: `https://github.com/raphatatto/gs-java-aquaguard`
- Deploy na nuvem: `https://aquaguard.example.com`
- Swagger UI: `https://aquaguard.example.com/swagger-ui.html`

## 📋 Sumário

1. [Visão Geral](#vis%C3%A3o-geral)
2. [Tecnologias & Dependências](#tecnologias--depend%C3%AAncias)
3. [Requisitos Funcionais](#requisitos-funcionais)
4. [Modelagem / Entidades](#modelagem--entidades)
5. [Endpoints (CRUD)](#endpoints-crud)
6. [Filtros, Paginação & Ordenação](#filtros-pagina%C3%A7%C3%A3o--ordena%C3%A7%C3%A3o)
7. [Swagger / OpenAPI](#swagger--openapi)
8. [Autenticação JWT](#autentica%C3%A7%C3%A3o-jwt)
9. [Como Rodar Localmente](#como-rodar-localmente)
10. [Deploy em Nuvem (Docker)](#deploy-em-nuvem-docker)
11. [Vídeos & Demonstração](#v%C3%ADdeos--demonstra%C3%A7%C3%A3o)

---

## Visão Geral

API para gerenciar **Usuários**, **Regiões** e **Alertas** de eventos extremos. Permite:

- CRUD completo
- Bean Validation nas requisições
- Relacionamentos 1:N (Região → Alertas)
- Paginação, ordenação e filtros
- Documentação interativa (Swagger)
- Autenticação simples via geração de JWT

---

## Tecnologias & Dependências

- Java 17 + Spring Boot 3
- Spring Data JPA (Oracle 12c)
- Hibernate
- Bean Validation (Jakarta)
- springdoc-openapi (Swagger UI)
- io.jsonwebtoken (JWT)
- Docker, Docker Compose

---

## Requisitos Funcionais

1. **API REST** seguindo boas práticas MVC.
2. **Persistência** em banco relacional (Oracle).
3. **Relacionamento** JPA 1→N (Região contém vários Alertas).
4. **Validação** de entrada (ex.: `@NotBlank`, `@Size`).
5. **Paginação / Ordenação / Filtros** nos endpoints GET.
6. **Swagger UI** em `/swagger-ui.html`.
7. **Geração de JWT** ao logar com email+senha.
8. **Deploy** containerizado em nuvem.

---

## Modelagem / Entidades

- **Usuario** (`tb_aqua_usuario`)
  - `id` (PK, sequencial)
  - `nomeUsuario`, `email` (único), `senha`, `telefone` (único), `permissao`

- **Regiao** (`tb_aqua_regiao`)
  - `id` (PK, sequencial)
  - `nome`, `cidade`, `coordenadasLat`, `coordenadasLng`

- **Alerta** (`tb_aqua_alerta`)
  - `id` (PK, sequencial)
  - `nivelRisco`, `descricao`, `data`
  - `regiaoId` (FK → Regiao)

---

## Endpoints (CRUD)

| Entidade | Verbo | URL                  | Descrição               |
| -------- | ----- | -------------------- | ----------------------- |
| Usuários | GET   | `/api/usuarios`      | Listar usuários (página)|
|          | GET   | `/api/usuarios/{id}` | Obter usuário por ID    |
|          | POST  | `/api/usuarios`      | Criar novo usuário      |
|          | PUT   | `/api/usuarios/{id}` | Atualizar usuário       |
|          | DELETE| `/api/usuarios/{id}` | Excluir usuário         |
| Regiões  | GET   | `/api/regioes`       | Listar regiões (página) |
|          | GET   | `/api/regioes/{id}`  | Obter região por ID     |
|          | POST  | `/api/regioes`       | Criar nova região       |
|          | PUT   | `/api/regioes/{id}`  | Atualizar região        |
|          | DELETE| `/api/regioes/{id}`  | Excluir região          |
| Alertas  | GET   | `/api/alertas`       | Listar alertas (página) |
|          | GET   | `/api/alertas/{id}`  | Obter alerta por ID     |
|          | POST  | `/api/alertas`       | Criar novo alerta       |
|          | PUT   | `/api/alertas/{id}`  | Atualizar alerta        |
|          | DELETE| `/api/alertas/{id}`  | Excluir alerta          |

---

## Filtros, Paginação & Ordenação

- Todos os **GET** suportam `Pageable`:
  ```
  GET /api/regioes?page=0&size=10&sort=cidade,asc
  ```
- Exemplo de filtro opcional em Região:
  ```
  GET /api/regioes?cidade=São
  ```
- No código: `Page<RegiaoDTO> listar(Pageable pg, Optional<String> cidade)`

---

## Swagger / OpenAPI

- `/v3/api-docs` → JSON da API
- `/swagger-ui.html` → UI interativa
- Anotações `@Operation`, `@ApiResponse` em cada controller

---

## Autenticação JWT

1. **Geração de token**
   - `POST /auth/login`
   - Corpo:
     ```json
     { "email":"user@ex.com", "senha":"senha123" }
     ```
   - Resposta:
     ```json
     { "token":"xxx.yyy.zzz", "type":"Bearer" }
     ```

2. **Sem filtros automáticos**
   - Endpoint público para geração de token.

---

## Como Rodar Localmente

1. Ajuste `application.properties` com sua conexão Oracle:
   ```properties
   spring.datasource.url=jdbc:oracle:thin:@//HOST:PORT/SERVICE
   spring.datasource.username=USUARIO
   spring.datasource.password=SENHA
   spring.jpa.hibernate.ddl-auto=update
   spring.jpa.show-sql=true

   jwt.expiration=3600000
   ```
2. Crie as sequences no Oracle:
   ```sql
   CREATE SEQUENCE USUARIO_SEQ START WITH 1 INCREMENT BY 1 NOCACHE;
   CREATE SEQUENCE REGIAO_SEQ   START WITH 1 INCREMENT BY 1 NOCACHE;
   CREATE SEQUENCE ALERTA_SEQ   START WITH 1 INCREMENT BY 1 NOCACHE;
   ```
3. Build e run:
   ```bash
   ./mvnw clean package
   java -jar target/aquaguard-api-java.jar
   ```
4. Acesse o Swagger em `http://localhost:8080/swagger-ui.html`.

---

## Deploy em Nuvem (Docker)

1. **Dockerfile** multistage pronto.
2. Deploy em Heroku/AWS/Azure.

---

## Vídeos & Demonstração

- Demonstração completa (≤10min).
- Pitch Java Advanced (≤3min).

Boa entrega! 🚀
