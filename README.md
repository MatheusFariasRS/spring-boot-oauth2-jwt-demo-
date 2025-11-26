# Spring Boot OAuth2 JWT Demo

Projeto de demonstração de autenticação e autorização usando **Spring Boot**, **OAuth2** e **JWT**.  
Este projeto é um fork do repositório de estudos da DevSuperior, com foco em mostrar como configurar um servidor de recursos com segurança baseada em tokens JWT.

## Tecnologias utilizadas

- **Java 17+** (confira a versão exata no `pom.xml`)
- **Spring Boot**
  - Spring Web
  - Spring Security (OAuth2 Resource Server / Authorization Server, conforme o branch)
- **JWT (JSON Web Token)**
- **Maven**
- Banco de dados (H2 / Postgres / outro — veja as configs em `application.properties`)

## Estrutura do projeto

O repositório possui ao menos o seguinte branch principal:

- `password-grant`: implementação de OAuth2 com fluxo **Password Grant** usando JWT.

Dentro do branch, você encontrará:

- `src/main/java/...`
  - config/: classes de configuração de segurança e JWT
  - entities/: entidades de domínio (User, Role, etc., se existirem)
  - repositories/: repositórios JPA
  - resources/: controladores REST expostos pela API
  - outras classes auxiliares (services, dto, etc.)
- `src/main/resources/`
  - application.properties ou application.yml com as configurações de:
    - porta da aplicação
    - banco de dados
    - chaves/segredos para geração/validação dos tokens JWT

Para detalhes exatos, navegue pelas pastas do branch `password-grant`.

## Funcionalidades

- Autenticação de usuários via OAuth2 com emissão de **JWT**.
- Proteção de endpoints REST com **Bearer Token**.
- Configuração de rotas públicas e privadas.
- Validação de token e extração de informações do usuário autenticado.

## Como executar o projeto

### 1. Clonar o repositório

```bash
git clone https://github.com/MatheusFariasRS/spring-boot-oauth2-jwt-demo.git
cd spring-boot-oauth2-jwt-demo
```

### 2. Escolher o branch

```bash
git checkout password-grant
```

### 3. Configurar o ambiente

Verifique o arquivo application.properties (ou application.yml) e ajuste se necessário:

- URL do banco de dados
- Usuário/senha do banco
- Porta da aplicação
- Segredo/chave do JWT

Exemplo (genérico):

```properties
server.port=8080

spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=create

jwt.secret=MINHA_CHAVE_SECRETA_AQUI
jwt.expiration=86400000
```

(Ajuste de acordo com o que já estiver configurado no projeto.)

### 4. Rodar a aplicação

Via Maven:

```bash
mvn spring-boot:run
```

Ou gerando o .jar:

```bash
mvn clean package
java -jar target/*.jar
```

A API deverá subir, por padrão, em http://localhost:8080 (confirme no application.properties).

## Como testar a autenticação

Os caminhos/endpoints exatos podem variar; ajuste conforme o que estiver nos controllers do projeto.

### 1. Obter um token JWT

Envie uma requisição de login (exemplo):

POST /oauth/token
Content-Type: application/x-www-form-urlencoded
Authorization: Basic <client_id:client_secret em Base64>

grant_type=password&username=<usuario>&password=<senha>

A resposta deve conter um access_token JWT:

{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "bearer",
  "expires_in": 86400,
  "scope": "read write"
}

### 2. Acessar endpoints protegidos

Use o token recebido no cabeçalho Authorization:

GET /usuarios
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

Se o token for válido e o usuário tiver permissão, o recurso será retornado.

## Estrutura de segurança (resumo)

- Configurações de segurança ficam, em geral, em classes como:
  - SecurityConfig
  - AuthorizationServerConfig
  - ResourceServerConfig
- O JWT é assinado usando um segredo configurado na aplicação.
- Perfis de usuário/roles definem quem pode acessar o quê.

## Pré-requisitos

- JDK instalado (versão compatível com o projeto, ver pom.xml)
- Maven instalado
- Banco de dados compatível (H2/Postgres/etc., conforme configuração)

## Possíveis melhorias

- Documentação com exemplos de requisições (Postman/Insomnia).
- Integração com banco de dados real (PostgreSQL/MySQL).
- Dockerfile e docker-compose para subir tudo com um comando.
- README em inglês para facilitar contribuição da comunidade.

## Referências

- Repositório original/forkado: devsuperior/spring-boot-oauth2-jwt-demo
- Documentação oficial:
  - Spring Security
  - Spring Boot
  - JWT (RFC 7519)

Este projeto é voltado para fins de estudo/demonstração de segurança com OAuth2 e JWT em aplicações Spring Boot.
