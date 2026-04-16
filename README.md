# News Portal - Rust Web Application

Um portal de notícias moderno, modular e seguro desenvolvido em **Rust**, com painel administrativo, gestão de usuários, categorias, e proteção contra ataques.

## 📋 Visão Geral

Este projeto é um sistema completo de portal de notícias construído com princípios de arquitetura limpa, focado em manutenibilidade, segurança e performance. Utiliza **PostgreSQL** como banco de dados e **Bootstrap** para as views.

## ✨ Funcionalidades

### Público
- 📰 Listagem de notícias com paginação
- 🔍 Busca de notícias por título/conteúdo
- 📂 Filtragem por categorias
- 👁️ Visualização completa de notícias (incrementa contador de views)
- 📝 Registro de visitantes por IP
- 🚫 Bloqueio automático de IPs com muitos erros 404 (proteção contra ataques)

### Administrativo (Painel Admin)
- 🔐 Autenticação segura (Login/Register)
- 👥 Gestão de usuários (apenas admins)
- 📰 CRUD completo de notícias
- 🏷️ CRUD de categorias
- 📊 Dashboard com estatísticas
- 🚫 Gestão de IPs bloqueados
- 🔒 Apenas usuários com role "admin" podem acessar

## 🏗️ Arquitetura do Projeto

```
news-portal/
├── Cargo.toml
├── .env
├── docker-compose.yml
├── migrations/
│   └── 001_initial_schema.sql
├── src/
│   ├── main.rs              # Entry point
│   ├── lib.rs               # Biblioteca interna
│   ├── config/
│   │   ├── mod.rs           # Configurações do aplicativo
│   │   └── database.rs      # Conexão com PostgreSQL
│   ├── models/
│   │   ├── mod.rs
│   │   ├── user.rs          # Modelo de usuário
│   │   ├── news.rs          # Modelo de notícia
│   │   ├── category.rs      # Modelo de categoria
│   │   ├── visitor.rs       # Modelo de visitante
│   │   └── ip_block.rs      # Modelo de IP bloqueado
│   ├── repositories/
│   │   ├── mod.rs
│   │   ├── user_repo.rs
│   │   ├── news_repo.rs
│   │   ├── category_repo.rs
│   │   ├── visitor_repo.rs
│   │   └── ip_block_repo.rs
│   ├── services/
│   │   ├── mod.rs
│   │   ├── auth_service.rs  # Lógica de autenticação
│   │   ├── news_service.rs  # Lógica de negócios de notícias
│   │   ├── visitor_service.rs # Rastreamento de visitantes
│   │   └── security_service.rs # Proteção contra ataques
│   ├── handlers/
│   │   ├── mod.rs
│   │   ├── auth_handler.rs
│   │   ├── news_handler.rs
│   │   ├── admin_handler.rs
│   │   └── error_handler.rs
│   ├── middleware/
│   │   ├── mod.rs
│   │   ├── auth_middleware.rs # Validação de sessão/admin
│   │   └── security_middleware.rs # Rate limiting e bloqueio de IP
│   ├── templates/
│   │   ├── base.html        # Template base com Bootstrap
│   │   ├── index.html
│   │   ├── news_detail.html
│   │   ├── login.html
│   │   ├── register.html
│   │   └── admin/
│   │       ├── dashboard.html
│   │       ├── news_list.html
│   │       ├── news_form.html
│   │       ├── categories.html
│   │       └── users.html
│   └── utils/
│       ├── mod.rs
│       ├── password.rs      # Hash de senhas
│       └── ip_extractor.rs  # Extração de IP real
└── tests/
    ├── integration_tests.rs
    └── unit_tests/
```

## 🛠️ Tecnologias Utilizadas

### Backend
- **Linguagem**: Rust (edição stable mais recente)
- **Framework Web**: Axum ou Actix-web (escolher um)
- **ORM**: SQLx (async, type-safe) ou Diesel
- **Template Engine**: Askama ou Tera
- **Autenticação**: JWT + Cookies seguros
- **Hash de Senha**: Argon2 ou bcrypt
- **Validação**: Validator crate

### Frontend
- **CSS Framework**: Bootstrap 5
- **JavaScript**: Vanilla JS ou Alpine.js para interatividade leve
- **Ícones**: Bootstrap Icons ou FontAwesome

### Banco de Dados
- **SGBD**: PostgreSQL 15+
- **Migrations**: SQLx migrate ou Diesel migrations

### Infraestrutura
- **Containerização**: Docker & Docker Compose
- **Variáveis de Ambiente**: dotenv

## 🚀 Requisitos

- Rust 1.70+
- PostgreSQL 15+
- Docker e Docker Compose (opcional, para desenvolvimento)
- cargo-make ou just (opcional, para automação)

## ⚙️ Configuração

### 1. Clone o repositório

```bash
git clone <repository-url>
cd news-portal
```

### 2. Configure as variáveis de ambiente

Crie um arquivo `.env` na raiz do projeto:

```env
# Database
DATABASE_URL=postgres://user:password@localhost:5432/news_portal

# Server
HOST=0.0.0.0
PORT=8080
RUST_LOG=info

# Security
JWT_SECRET=your-super-secret-jwt-key-change-in-production
SESSION_COOKIE_NAME=news_portal_session
COOKIE_SECURE=false

# Security Settings
MAX_404_BEFORE_BLOCK=50
BLOCK_DURATION_MINUTES=60
VISITOR_TRACKING_ENABLED=true

# Admin Initial Setup
ADMIN_EMAIL=admin@example.com
ADMIN_PASSWORD=ChangeMe123!
```

### 3. Subir o banco de dados (com Docker)

```bash
docker-compose up -d postgres
```

Ou instale o PostgreSQL localmente e crie o banco:

```sql
CREATE DATABASE news_portal;
CREATE USER user WITH PASSWORD 'password';
GRANT ALL PRIVILEGES ON DATABASE news_portal TO user;
```

### 4. Executar migrations

```bash
sqlx migrate run
# ou
cargo sqlx migrate run
```

### 5. Build e Run

```bash
# Development
cargo run

# Production
cargo build --release
./target/release/news-portal
```

Acesse `http://localhost:8080`

## 🔐 Segurança

### Proteção Contra Ataques
- **Rate Limiting**: Limite de requisições por IP
- **Bloqueio Automático**: IPs com mais de N erros 404 são bloqueados temporariamente
- **SQL Injection Prevention**: Queries parametrizadas via SQLx/Diesel
- **XSS Protection**: Escape automático nas templates
- **CSRF Protection**: Tokens CSRF em formulários
- **Password Hashing**: Argon2/bcrypt para senhas
- **Session Security**: Cookies HttpOnly, Secure, SameSite

### Controle de Acesso
- Middleware de autenticação para rotas protegidas
- Verificação de role "admin" para acesso ao painel
- Session timeout configurável

## 📦 Módulos Principais

### 1. Autenticação (`auth_service`)
- Registro de novos usuários (apenas admin pode criar)
- Login com validação de credenciais
- Geração e validação de JWT tokens
- Logout e invalidação de sessão

### 2. Notícias (`news_service`)
- CRUD completo de notícias
- Associação com categorias
- Slug generation para URLs amigáveis
- Contador de visualizações
- Status (rascunho/publicado)

### 3. Visitantes (`visitor_service`)
- Tracking de visitantes por IP
- Registro de páginas visitadas
- Estatísticas de acesso
- Identificação de padrões suspeitos

### 4. Segurança (`security_service`)
- Monitoramento de erros 404
- Bloqueio temporário de IPs maliciosos
- Lista negra de IPs
- Rate limiting configurável

## 🧪 Testes

```bash
# Rodar todos os testes
cargo test

# Testes unitários
cargo test --lib

# Testes de integração
cargo test --test integration_tests

# Com coverage (requer cargo-tarpaulin)
cargo tarpaulin --out Html
```

## 📝 Scripts Úteis

```bash
# Format code
cargo fmt

# Lint code
cargo clippy -- -D warnings

# Check without building
cargo check

# Build release
cargo build --release
```

## 🔄 Deploy em Produção

### Recomendações
1. Use `RUST_LOG=warn` ou `error` em produção
2. Habilite `COOKIE_SECURE=true` com HTTPS
3. Altere todas as chaves secretas no `.env`
4. Use um reverse proxy (Nginx/Traefik)
5. Configure backup automático do PostgreSQL
6. Monitore logs e métricas

### Docker Production

```bash
docker-compose -f docker-compose.prod.yml up -d
```

## 📄 License

MIT License - veja [LICENSE](LICENSE) para detalhes.

## 🤝 Contribuição

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📞 Suporte

Para issues e dúvidas, abra uma issue no repositório.

---

**Desenvolvido com ❤️ em Rust**
