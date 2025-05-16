SpringBootJWT - API com MongoDB e Autenticação JWT

Este projeto é uma API RESTful construída com Spring Boot, utilizando MongoDB para persistência e JWT (JSON Web Token) para autenticação.

──────────────────────────────────────────────
REQUISITOS:
──────────────────────────────────────────────
- Java 17 ou superior
- Maven
- MongoDB instalado e rodando localmente
- IntelliJ (ou outra IDE Java)
- Postman (para testar os endpoints)

──────────────────────────────────────────────
COMO RODAR O PROJETO:
──────────────────────────────────────────────

1. Certifique-se de que o MongoDB está rodando:
   - Porta: 27017
   - Banco de dados: api_escola

2. Verifique o arquivo application.properties:
   (localizado em src/main/resources)

   spring.application.name=SpringBootJWT
   spring.data.mongodb.uri=mongodb://localhost:27017/api_escola
   spring.security.jwt.secret=MinhaChaveSuperSeguraJWT1234567890!
   spring.security.jwt.expiration=86400000
   logging.level.org.springframework.web=DEBUG
   logging.level.org.hibernate=ERROR

   ⚠️ A chave JWT deve ter no mínimo 32 caracteres e não pode conter caracteres inválidos.

3. Compile e rode o projeto:

   Com Maven:
   ./mvnw spring-boot:run

   Ou no IntelliJ:
   - Clique com o botão direito na classe SpringBootJwtApplication.java
   - Escolha "Run"

──────────────────────────────────────────────
ENDPOINTS DISPONÍVEIS:
──────────────────────────────────────────────

AUTENTICAÇÃO (PÚBLICO)

POST /auth/register
→ Cadastra um novo usuário
Body JSON:
{
  "email": "exemplo@email.com",
  "senha": "123456"
}

POST /auth/login
→ Autentica e retorna um token JWT
Body JSON:
{
  "email": "exemplo@email.com",
  "senha": "123456"
}
Resposta:
{
  "token": "eyJhbGciOiJIUzI1NiJ9..."
}

──────────────────────────────────────────────

ENDPOINTS PROTEGIDOS (EXIGEM TOKEN JWT)

→ Adicione o token no header:
Authorization: Bearer SEU_TOKEN_AQUI

GET /alunos
→ Lista todos os alunos

POST /alunos
→ Cadastra um novo aluno
Body JSON:
{
  "nome": "João",
  "nota1": 8.0,
  "nota2": 9.0,
  "email": "aluno@email.com"
}

PUT /alunos/{id}
→ Atualiza um aluno pelo ID

DELETE /alunos/{id}
→ Remove um aluno pelo ID

──────────────────────────────────────────────
TESTANDO COM POSTMAN:
──────────────────────────────────────────────

1. Faça POST em /auth/register
2. Faça POST em /auth/login e copie o token JWT da resposta
3. Em cada chamada protegida (/alunos), use o cabeçalho:
   Authorization: Bearer <seu_token>

──────────────────────────────────────────────
ESTRUTURA DO PROJETO:
──────────────────────────────────────────────

src/
├── controller/         → AuthController, AlunoController
├── model/              → Usuario, NotaAluno
├── repository/         → UsuarioRepository, NotaAlunoRepository
├── service/            → AuthService
├── security/           → JwtUtil, JwtFilter, SecurityConfig
└── SpringBootJwtApplication.java

──────────────────────────────────────────────
OBS:
──────────────────────────────────────────────
- Apenas os endpoints /auth/** são públicos
- Todo o restante exige autenticação com token JWT
- MongoDB deve estar rodando antes de iniciar o projeto
- Token precisa ser gerado no login e usado nas próximas requisições

──────────────────────────────────────────────
