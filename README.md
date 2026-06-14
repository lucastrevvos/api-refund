# API Refund

API REST para gerenciamento de solicitações de reembolso com autenticação, perfis de acesso e upload de comprovantes.

## Visão geral

A **API Refund** é um backend desenvolvido para organizar o fluxo de reembolsos corporativos, conectando colaboradores que registram despesas a gestores que acompanham e consultam as solicitações.

O projeto centraliza cadastro de usuários, autenticação, controle de permissões, registro de reembolsos e envio de comprovantes em uma API simples, objetiva e pronta para integração com aplicações frontend ou ferramentas internas.

## Problema

Processos de reembolso feitos de forma manual podem dificultar a organização das despesas, a consulta de solicitações e a separação de responsabilidades entre colaboradores e gestores.

Uma API dedicada permite estruturar esse fluxo com regras claras, dados persistidos e endpoints específicos para cada etapa do processo.

## Solução

O projeto oferece uma API REST em Node.js e TypeScript para:

- cadastrar usuários com diferentes perfis de acesso;
- autenticar usuários com JWT;
- permitir que colaboradores registrem solicitações de reembolso;
- permitir que gestores consultem solicitações com paginação e filtro;
- armazenar comprovantes enviados por upload;
- validar dados de entrada de forma consistente antes da persistência.

## Funcionalidades

- Cadastro de usuários com nome, e-mail, senha e perfil.
- Autenticação com e-mail e senha.
- Geração de token JWT com identificação do usuário e perfil de acesso.
- Controle de rotas privadas por autenticação.
- Autorização por perfil de usuário:
  - `employee`: criação de reembolsos e envio de comprovantes;
  - `manager`: listagem de reembolsos.
- Criação de solicitações de reembolso com categoria, valor, descrição e comprovante associado.
- Consulta de reembolso por ID.
- Listagem paginada de reembolsos com filtro por nome do usuário.
- Upload de arquivos de comprovante nos formatos JPEG, JPG e PNG.
- Disponibilização estática dos arquivos enviados pela rota `/uploads`.
- Tratamento centralizado de erros da aplicação.
- Validação de payloads, parâmetros e arquivos com Zod.

## Stack

- **Node.js**
- **TypeScript**
- **Express**
- **Prisma ORM**
- **SQLite**
- **JWT** para autenticação
- **bcrypt** para hash de senhas
- **Zod** para validação de dados
- **Multer** para upload de arquivos
- **CORS**
- **tsx** para execução em ambiente de desenvolvimento

## Arquitetura

A aplicação segue uma organização em camadas simples e direta, com responsabilidades separadas por módulos:

```txt
src/
├── configs/        # Configurações de autenticação e upload
├── controllers/    # Regras de entrada e saída das rotas
├── database/       # Instância do Prisma Client
├── middlewares/    # Autenticação, autorização e tratamento de erros
├── providers/      # Serviços auxiliares, como persistência de arquivos
├── routes/         # Definição e composição das rotas HTTP
├── types/          # Extensões de tipos do Express
├── utils/          # Utilitários compartilhados
├── app.ts          # Configuração da aplicação Express
└── server.ts       # Inicialização do servidor HTTP
```

O banco de dados é modelado com Prisma e utiliza duas entidades principais:

- **User**: representa os usuários da aplicação, com perfil `employee` ou `manager`.
- **Refunds**: representa as solicitações de reembolso vinculadas a um usuário.

As rotas públicas concentram cadastro e autenticação. As demais rotas passam pelo middleware de autenticação JWT e, quando necessário, por autorização baseada em perfil.

## Como rodar

### Pré-requisitos

- Node.js instalado
- npm instalado

### Passo a passo

1. Clone o repositório:

```bash
git clone <URL_DO_REPOSITORIO>
```

2. Acesse a pasta do projeto:

```bash
cd api-refund
```

3. Instale as dependências:

```bash
npm install
```

4. Gere o Prisma Client:

```bash
npx prisma generate
```

5. Execute a aplicação em modo de desenvolvimento:

```bash
npm run dev
```

6. Acesse a API em:

```txt
http://localhost:3333
```

## Configuração

O projeto está configurado para execução local com SQLite e Prisma.

Configurações relevantes da aplicação:

| Configuração | Onde fica | Finalidade |
| --- | --- | --- |
| Banco SQLite | `prisma/schema.prisma` | Define o datasource local da aplicação. |
| Porta da API | `src/server.ts` | Define a porta HTTP usada pelo servidor. |
| JWT | `src/configs/auth.ts` | Define parâmetros de assinatura e expiração do token. |
| Uploads | `src/configs/upload.ts` | Define pastas, estratégia de armazenamento, tamanho máximo e tipos aceitos. |

Não há variáveis de ambiente obrigatórias para executar a versão local atual.

## Decisões técnicas

- **TypeScript** foi utilizado para aumentar previsibilidade no desenvolvimento e melhorar a manutenção do código.
- **Express** foi escolhido pela simplicidade na criação de APIs REST e pela flexibilidade na composição de middlewares.
- **Prisma ORM** centraliza o acesso ao banco de dados e mantém o modelo relacional descrito em um schema versionável.
- **SQLite** facilita a execução local do projeto e reduz dependências externas para demonstração e desenvolvimento.
- **JWT** permite autenticação stateless e transporte simples da identidade do usuário entre requisições.
- **bcrypt** protege senhas por meio de hash antes da persistência.
- **Zod** garante validação explícita dos dados recebidos nas rotas.
- **Multer** gerencia o recebimento de arquivos enviados pelos usuários.
- **Middlewares dedicados** separam autenticação, autorização e tratamento de erros, mantendo os controllers mais focados no fluxo de cada recurso.

## Status

O projeto está em uma versão funcional para execução local, com fluxo principal de autenticação, controle de acesso, cadastro de usuários, criação e consulta de reembolsos, além de upload de comprovantes.

A API está estruturada como uma peça de portfólio backend, demonstrando a construção de um serviço REST com persistência, segurança básica, validação e organização em camadas.

## Roadmap

Evoluções planejadas para o produto:

- Documentação interativa dos endpoints com Swagger ou OpenAPI.
- Suporte a migrations versionadas para evolução do schema do banco.
- Testes automatizados para fluxos críticos da API.
- Configuração por variáveis de ambiente para diferentes ambientes de execução.
- Fluxo de aprovação de reembolsos por gestores.
- Campos adicionais de status para acompanhamento das solicitações.
- Integração com armazenamento externo para comprovantes.
- Preparação para deploy em ambiente cloud.

## O que este projeto demonstra

Este projeto evidencia competências importantes para desenvolvimento backend, incluindo:

- criação de APIs REST com Node.js, Express e TypeScript;
- modelagem de dados com Prisma ORM;
- autenticação com JWT;
- autorização por perfil de usuário;
- validação de dados com Zod;
- upload e gerenciamento de arquivos com Multer;
- estruturação de middlewares reutilizáveis;
- tratamento centralizado de erros;
- organização de projeto backend em camadas;
- construção de uma aplicação com foco em clareza, manutenção e integração com clientes externos.
