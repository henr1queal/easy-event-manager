# Easy Event Manager — Plataforma de Gestão Operacional

Sistema web para empresas que organizam viagens, hospedagens e eventos corporativos. Centraliza clientes, fornecedores, logística de deslocamento, hospedagem, financeiro e equipe em um único painel, com controle de acesso por perfil.

---

## Problema que resolve

Empresas desse segmento costumam operar com planilhas, e-mails e ferramentas desconectadas. Isso dificulta rastrear custos por evento, acompanhar pagamentos a palestrantes e fornecedores, controlar notas fiscais e garantir que cada colaborador acesse apenas o que precisa.

O Easy Event Manager unifica essas operações: cada evento concentra dados do cliente, palestrantes, parceiros, fornecedores, transporte, hospedagem e informações financeiras — com histórico, status e relatórios.

---

## Funcionalidades

### Eventos
- Cadastro de eventos presenciais, online e híbridos
- Vinculação de clientes, temas, palestrantes, parceiros e fornecedores
- Controle de status, prazos de pagamento e valores (total, palestrantes, parceiros)
- Upload e gestão de notas fiscais (PDF), com datas de faturamento e lembretes
- Envio de e-mails de cobrança vinculados às notas fiscais

### Logística de viagem e hospedagem
- **Transporte:** avião, ônibus (mesma cidade e intermunicipal), táxi e Uber — vinculados ao evento
- **Hospedagem:** check-in/check-out, endereço completo e observações por evento

### Cadastros operacionais
- **Clientes:** dados cadastrais, CNPJ, endereço, responsável, dados bancários e PIX
- **Fornecedores:** categorias customizáveis e histórico por evento
- **Palestrantes e parceiros:** perfis completos com vínculo a eventos e controle de pagamento
- **Bancos e tipos de organização** como insumos de apoio ao cadastro

### Financeiro
- Controle de caixa com atualização de saldo
- Registro de despesas com status (aberta / a pagar)
- Consolidação mensal de valores
- Visão de totais em aberto e a pagar

### Equipe e permissões
- Cadastro de colaboradores com fluxo de aprovação (pendente → ativo)
- Perfis de cargo (ex.: administrador) e permissões granulares por funcionalidade
- Middleware de autorização em todas as rotas sensíveis
- Autenticação com Laravel Jetstream (2FA, sessões, perfil)

### Estatísticas
- Faturamento total de eventos concluídos
- Temas e palestrantes mais contratados
- Consulta de faturamento por intervalo de datas

---

## Stack tecnológica

| Camada        | Tecnologia                          |
|---------------|-------------------------------------|
| Backend       | PHP 8.1+, Laravel 10                |
| Autenticação  | Laravel Jetstream + Fortify         |
| Frontend      | Blade, Livewire 3, Bootstrap Icons  |
| Banco de dados| MySQL                               |
| API / tokens  | Laravel Sanctum                     |
| Testes        | PHPUnit                             |

---

## Arquitetura

```
app/
├── Http/
│   ├── Controllers/     # CRUD e regras de negócio por domínio
│   └── Middleware/      # Autenticação e CheckPermission (RBAC)
├── Models/              # Event, Customer, Hosting, Transport*, Financial…
└── Providers/           # Permissões e boot da aplicação

database/migrations/     # Schema relacional com FKs entre entidades
resources/views/         # Interfaces Blade por módulo
routes/web.php           # Rotas protegidas por middleware de permissão
```

**Destaques de implementação:**
- Relacionamentos Eloquent complexos (eventos ↔ palestrantes/fornecedores via pivot com preço e status de pagamento)
- Middleware customizado `CheckPermission` para RBAC baseado em features por usuário
- Upload de documentos fiscais com armazenamento em disco
- Paginação e ordenação customizada na listagem de eventos por status e prazo

---

## Requisitos

- PHP >= 8.1
- Composer
- MySQL >= 5.7
- Node.js e npm (assets front-end)

---

## Instalação

```bash
# Clonar o repositório
git clone https://github.com/henr1queal/easy-event-manager.git
cd easy-event-manager

# Dependências PHP
composer install

# Dependências front-end
npm install && npm run build

# Ambiente
cp .env.example .env
php artisan key:generate

# Configurar DB_CONNECTION, DB_HOST, DB_DATABASE, DB_USERNAME e DB_PASSWORD no .env

# Banco de dados
php artisan migrate

# Servidor local
php artisan serve
```

Acesse `http://localhost:8000`. O primeiro usuário administrador deve ser configurado via seed ou diretamente no banco (cargo com `position_id = 1`).

---

## Módulos e rotas principais

| Módulo         | Rota base        |
|----------------|------------------|
| Dashboard      | `/`              |
| Clientes       | `/clientes`      |
| Fornecedores   | `/fornecedores`  |
| Parceiros      | `/parceiros`     |
| Palestrantes   | `/palestrantes`  |
| Eventos        | `/eventos`       |
| Financeiro     | `/financeiro`    |
| Colaboradores  | `/colaboradores` |
| Estatísticas   | `/estatisticas`  |

Todas as rotas (exceto login/registro) exigem autenticação e permissão específica.

---

## Contato

Henrique Silva — [henriquersilva.al@gmail.com](mailto:henriquersilva.al@gmail.com)
