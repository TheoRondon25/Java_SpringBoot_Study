# 📝 TodoList API

API REST de gerenciamento de tarefas desenvolvida durante o **Minicurso de Java com Spring Boot** da [Rocketseat](https://rocketseat.com.br).

🔗 **Deploy:** [https://todolist-rocketjava-29ch.onrender.com](https://todolist-rocketjava-29ch.onrender.com)

---

## 🚀 Tecnologias

- **Java 17**
- **Spring Boot 4.1.0**
- **Maven**
- **Spring Data JPA**
- **H2 Database** (banco em memória para testes)
- **Lombok**
- **BCrypt** (`at.favre.lib`) — hash de senhas
- **Docker**

---

## 📌 Funcionalidades

- Cadastro de usuários com senha criptografada (BCrypt)
- Autenticação via **HTTP Basic Auth**
- Criação, listagem e atualização de tarefas
- Validação de datas (início e término devem ser futuros; início antes do término)
- Cada usuário acessa e edita apenas as próprias tarefas
- Título da tarefa limitado a 50 caracteres

---

## 🗂️ Estrutura do Projeto

```
src/
└── main/
    └── java/br/com/theorondon/todolist/
        ├── filter/
        │   └── FilterTaskAuth.java      # Filtro de autenticação Basic Auth
        ├── task/
        │   ├── TaskController.java      # Endpoints de tarefas
        │   ├── TaskModel.java           # Entidade da tarefa
        │   └── ITaskRepository.java     # Repositório JPA
        ├── user/
        │   ├── UserController.java      # Endpoints de usuários
        │   ├── UserModel.java           # Entidade do usuário
        │   └── IUserRepository.java     # Repositório JPA
        └── utils/
            └── Utils.java               # Utilitários (ex: cópia de propriedades)
```

---

## 🔌 Endpoints

### Usuários

#### Criar usuário
```
POST /users/
Content-Type: application/json

{
  "name": "Theo",
  "username": "theo",
  "password": "123456"
}
```

---

### Tarefas

> Todos os endpoints de tarefas requerem **autenticação Basic Auth** (username + password).

#### Criar tarefa
```
POST /tasks/
Authorization: Basic <base64(username:password)>
Content-Type: application/json

{
  "title": "Estudar Spring Boot",
  "description": "Assistir as aulas do minicurso",
  "startAt": "2025-10-20T12:00:00",
  "endAt": "2025-10-20T15:00:00",
  "priority": "ALTA"
}
```

#### Listar tarefas do usuário
```
GET /tasks/
Authorization: Basic <base64(username:password)>
```

#### Atualizar tarefa
```
PUT /tasks/{id}
Authorization: Basic <base64(username:password)>
Content-Type: application/json

{
  "title": "Novo título",
  "priority": "MÉDIA"
}
```

---

## 🗃️ Modelo da Tarefa

| Campo | Tipo | Descrição |
|---|---|---|
| `id` | UUID | Identificador único |
| `title` | String (max 50) | Título da tarefa |
| `description` | String | Descrição detalhada |
| `startAt` | LocalDateTime | Data/hora de início |
| `endAt` | LocalDateTime | Data/hora de término |
| `priority` | String | Prioridade (ex: ALTA, MÉDIA, BAIXA) |
| `idUser` | UUID | Usuário dono da tarefa |
| `createdAt` | LocalDateTime | Data de criação (automático) |

---

## 🔐 Autenticação

A API usa **HTTP Basic Authentication**. As credenciais devem ser enviadas no header:

```
Authorization: Basic <base64(username:password)>
```

O filtro `FilterTaskAuth` intercepta todas as requisições em `/tasks/**`, valida as credenciais e injeta o `idUser` na requisição.

---

## 🏃 Rodando localmente

### Pré-requisitos

- Java 17+
- Maven

### Passos

```bash
# Clone o repositório
git clone https://github.com/TheoRondon25/Java_SpringBoot_Study.git

# Entre na pasta do projeto
cd Java_SpringBoot_Study

# Execute o projeto
./mvnw spring-boot:run
```

A API estará disponível em `http://localhost:8080`.

O console do H2 pode ser acessado em `http://localhost:8080/h2-console`.

---

## 🐳 Rodando com Docker

```bash
# Build da imagem
docker build -t todolist .

# Executar o container
docker run -p 8080:8080 todolist
```

---

## ☁️ Deploy

A API está publicada no **Render** e pode ser acessada em:

> [https://todolist-rocketjava-29ch.onrender.com](https://todolist-rocketjava-29ch.onrender.com)

> ⚠️ **Atenção:** o plano gratuito do Render coloca o serviço em modo de espera após inatividade. A primeira requisição pode demorar alguns segundos para responder enquanto o servidor reinicia.

---

## 📚 Aprendizados do Minicurso

- Criação de projetos Spring Boot com Maven
- Mapeamento de entidades com JPA/Hibernate
- Criação de controllers REST (`@RestController`, `@RequestMapping`)
- Filtros de servlet para autenticação
- Criptografia de senhas com BCrypt
- Banco de dados em memória com H2
- Containerização com Docker
- Deploy de aplicações Java no Render

---

## 👨‍💻 Autor

**Theo Rondon**  
[Linkedin](https://www.linkedin.com/in/théo-rondon-b7259726b)
