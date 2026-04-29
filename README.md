# next-fullstack-boilerplate

Boilerplate fullstack pessoal baseado em Next.js com App Router, Prisma, Vitest, Docker e CI configurado. Criado para eliminar setup repetitivo e garantir consistência entre projetos.

> **Importante**: Este é um boilerplate, não um projeto pronto. Ele fornece a estrutura, configurações e convenções base. Você vai adicionar models, componentes, rotas e lógica de negócio por cima disso.

---

## Índice

- [Stack](#stack)
- [Pré-requisitos](#pré-requisitos)
- [Como usar este boilerplate](#como-usar-este-boilerplate)
- [Primeiros passos após clonar](#primeiros-passos-após-clonar)
- [Estrutura do projeto](#estrutura-do-projeto)
- [Variáveis de ambiente](#variáveis-de-ambiente)
- [Banco de dados](#banco-de-dados)
- [Autenticação](#autenticação)
- [Testes](#testes)
- [CI/CD](#cicd)
- [Scripts disponíveis](#scripts-disponíveis)
- [O que você precisa criar ao usar em um projeto real](#o-que-você-precisa-criar-ao-usar-em-um-projeto-real)
- [Como manter o boilerplate atualizado](#como-manter-o-boilerplate-atualizado)
- [Convenções de commit](#convenções-de-commit)

---

## Stack

| Categoria | Tecnologia | Versão | Por quê |
|---|---|---|---|
| Framework | Next.js (App Router) | 16.x | SSR, SSG, API Routes, file-based routing tudo em um |
| Linguagem | TypeScript | 5.x | Tipagem estática, autocomplete, segurança em refactors |
| Estilização | Tailwind CSS | 4.x | Utility-first, sem CSS files extras, consistência visual |
| ORM | Prisma | 7.x | Tipagem automática do banco, migrations, Prisma Studio |
| Banco de dados | PostgreSQL | 16 | Relacional, robusto, padrão do mercado |
| Containerização | Docker + Docker Compose | - | Banco local sem instalar Postgres na máquina |
| Testes | Vitest + Testing Library | 4.x | Mais rápido que Jest, integração nativa com Vite/ESM |
| Formatação | Prettier | 3.x | Formato de código consistente e automático |
| Linting | ESLint | 9.x | Detecta erros e más práticas em tempo de desenvolvimento |
| CI | GitHub Actions | - | Roda lint, format check e testes automaticamente em todo push |
| Runtime de scripts | tsx | 4.x | Executa arquivos TypeScript diretamente (seeds, scripts) |

---

## Pré-requisitos

Antes de usar este boilerplate, você precisa ter instalado na sua máquina:

- **Node.js** 20 ou superior — [nodejs.org](https://nodejs.org)
- **pnpm** — `npm install -g pnpm`
- **Docker** e **Docker Compose** — [docs.docker.com](https://docs.docker.com/get-docker)
- **Git** — [git-scm.com](https://git-scm.com)

---

## Como usar este boilerplate

Este repositório é um **GitHub Template**. Para usá-lo em um novo projeto:

### Opção A — Via GitHub (recomendado)

1. Acesse o repositório no GitHub
2. Clique em **"Use this template"** → **"Create a new repository"**
3. Dê um nome ao novo repositório e crie
4. Clone o novo repositório na sua máquina

### Opção B — Via degit (sem histórico do boilerplate)

```bash
npx degit seu-usuario/next-fullstack-boilerplate nome-do-projeto
cd nome-do-projeto
git init
git remote add origin https://github.com/seu-usuario/nome-do-projeto.git
```

> **Nunca** clone diretamente este repositório para iniciar um projeto. Use uma das opções acima para manter o histórico de commits limpo.

---

## Primeiros passos após clonar

Execute os passos abaixo **na ordem** sempre que iniciar um projeto a partir deste boilerplate:

```bash
# 1. Instalar dependências
pnpm install

# 2. Criar o arquivo de ambiente
cp .env.example .env
# Edite o .env com os valores corretos para o seu projeto

# 3. Subir o banco de dados local
docker compose up -d

# 4. Rodar as migrations (após criar seus models no schema.prisma)
pnpm db:migrate

# 5. Iniciar o servidor de desenvolvimento
pnpm dev
```

Acesse [http://localhost:3000](http://localhost:3000).

---

## Estrutura do projeto

```
next-fullstack-boilerplate/
├── .github/
│   └── workflows/
│       └── ci.yml              # Pipeline de CI (lint, format, testes)
├── prisma/
│   ├── schema.prisma           # Definição dos models do banco
│   ├── migrations/             # Histórico de migrations (gerado pelo Prisma)
│   └── seed.ts                 # Script de seed do banco (você cria)
├── public/                     # Arquivos estáticos públicos (favicon, og images, etc)
├── src/
│   ├── app/                    # Rotas da aplicação (App Router do Next.js)
│   │   ├── layout.tsx          # Layout raiz — fonte, metadata global, providers
│   │   ├── page.tsx            # Página inicial (/)
│   │   └── globals.css         # (ver src/styles) — importado pelo layout
│   ├── assets/                 # Arquivos estáticos usados no código (SVGs, imagens, fontes locais)
│   ├── components/             # Componentes React reutilizáveis
│   │   └── ui/                 # Componentes de UI genéricos (Button, Input, Modal, etc)
│   ├── generated/              # Código gerado automaticamente pelo Prisma — NÃO editar
│   │   └── prisma/             # Prisma Client tipado (gerado via pnpm db:generate)
│   ├── lib/                    # Configurações de bibliotecas e integrações externas
│   │   └── prisma.ts           # Instância singleton do Prisma Client
│   ├── providers/              # React Context Providers
│   │   └── (ex: QueryProvider) # Ex: React Query, tema, auth context
│   ├── services/               # Funções de acesso à API ou ao banco
│   │   └── (ex: user.ts)       # Ex: getUser(), createPost() — separa fetch da UI
│   ├── styles/                 # Arquivos CSS globais
│   │   └── globals.css         # Reset, variáveis CSS, configuração do Tailwind
│   ├── tests/                  # Arquivos de teste
│   │   ├── setup.ts            # Setup global do Vitest (importa jest-dom)
│   │   └── example.test.ts     # Teste placeholder — substitua pelos seus
│   ├── utils/                  # Funções utilitárias puras e genéricas
│   │   └── (ex: format.ts)     # Ex: formatDate(), formatCurrency(), cn() do shadcn
│   └── validations/            # Schemas de validação com Zod
│       └── (ex: user.ts)       # Ex: schemas de formulários, reutilizados no front e back
├── .env                        # Variáveis de ambiente locais — NÃO commitar
├── .env.example                # Template das variáveis de ambiente — commitar sempre
├── .gitignore                  # Arquivos ignorados pelo Git
├── .prettierignore             # Arquivos ignorados pelo Prettier
├── .prettierrc                 # Configuração do Prettier
├── docker-compose.yml          # Sobe o PostgreSQL local
├── eslint.config.mjs           # Configuração do ESLint
├── next.config.ts              # Configuração do Next.js
├── package.json                # Dependências e scripts
├── postcss.config.mjs          # Configuração do PostCSS (necessário pro Tailwind v4)
├── prisma.config.ts            # Configuração do Prisma 7 (connection URL, paths)
├── tsconfig.json               # Configuração do TypeScript
└── vitest.config.ts            # Configuração do Vitest
```

### Por que essa estrutura?

- **`app/`** contém apenas rotas. Lógica, componentes e utilitários ficam fora dela para facilitar reuso e testes.
- **`components/`** é para componentes reutilizáveis. Componentes específicos de uma rota ficam dentro da própria pasta de rota em `app/`.
- **`services/`** separa a camada de acesso a dados da UI. Um componente nunca faz `fetch` diretamente — ele chama uma função de `services/`.
- **`validations/`** centraliza os schemas Zod. O mesmo schema valida o formulário no frontend e o body da requisição no backend.
- **`providers/`** separa providers de componentes visuais — facilita identificar o que é contexto global.
- **`lib/`** é para configurações de libs terceiras, não para lógica de negócio.

---

## Variáveis de ambiente

O arquivo `.env.example` contém todas as variáveis necessárias com valores de exemplo. **Nunca commite o `.env`**.

```env
# Database
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/myapp"

# App
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```

> Variáveis prefixadas com `NEXT_PUBLIC_` são expostas no navegador. Nunca coloque secrets nelas.

Ao adicionar uma nova variável de ambiente no projeto:
1. Adicione no `.env` com o valor real
2. Adicione no `.env.example` com um valor de exemplo ou vazio
3. Documente para que ela serve aqui neste README

---

## Banco de dados

### Subir o banco local

```bash
docker compose up -d
```

Isso sobe um container PostgreSQL na porta `5432` com:
- **Usuário**: `postgres`
- **Senha**: `postgres`
- **Database**: `myapp`

> Ao usar em um projeto real, altere os valores no `docker-compose.yml` e no `.env`.

### Parar o banco

```bash
docker compose down
```

> Use sempre `docker compose down` antes de desligar a máquina para evitar processos zumbis ocupando a porta 5432.

### Workflow de migrations

```bash
# Criar e aplicar uma nova migration após editar o schema.prisma
pnpm db:migrate

# Regenerar o Prisma Client após mudanças no schema (sem criar migration)
pnpm db:generate

# Abrir o Prisma Studio (interface visual do banco)
pnpm db:studio

# Rodar o seed
pnpm db:seed

# Resetar o banco (apaga tudo e reaplica migrations + seed)
pnpm db:reset
```

### Após resetar volumes do Docker

Se você apagou os volumes do Docker, as migrations precisam ser reaplicadas:

```bash
docker compose down -v        # Para e apaga volumes
docker compose up -d          # Sobe novamente
pnpm db:migrate               # Reaplica as migrations
pnpm db:seed                  # Roda o seed (se necessário)
```

---

## Autenticação

Este boilerplate **não inclui autenticação configurada por padrão**. A escolha de auth varia muito por projeto. Recomendações:

| Opção | Quando usar |
|---|---|
| **Better Auth** | Projetos que precisam de auth completa com sessões, OAuth e Prisma. Substituto moderno do Lucia. |
| **JWT manual** | APIs puras, projetos simples ou quando você quer controle total. |
| **Clerk** | Quando DX é prioridade e o projeto pode ter custo de serviço externo. |

### Adicionando Better Auth

```bash
pnpm add better-auth
```

Consulte a [documentação do Better Auth](https://better-auth.com) para configurar os models no `schema.prisma` e o handler em `app/api/auth/[...all]/route.ts`.

---

## Testes

O projeto usa **Vitest** com **Testing Library**.

```bash
# Rodar todos os testes uma vez
pnpm test --run

# Rodar em modo watch (re-executa ao salvar)
pnpm test

# Abrir a interface visual do Vitest
pnpm test:ui
```

### Onde criar testes

- Testes de componentes: `src/tests/components/`
- Testes de utils/validations: `src/tests/unit/`
- Testes de integração: `src/tests/integration/`

### Convenção de nomenclatura

```
NomeDoArquivo.test.ts     # teste unitário ou de integração
NomeDoComponente.test.tsx # teste de componente React
```

---

## CI/CD

O pipeline de CI roda automaticamente em todo push e pull request para `main` e `develop`.

### O que o CI verifica

1. **Lint** — `pnpm lint` — garante que não há erros de ESLint
2. **Format check** — `pnpm format:check` — garante que o código está formatado com Prettier
3. **Testes** — `pnpm test --run` — garante que todos os testes passam

### Antes de fazer push

Para evitar falhas no CI, rode localmente:

```bash
pnpm lint
pnpm format
pnpm test --run
```

---

## Scripts disponíveis

| Script | Comando | O que faz |
|---|---|---|
| Desenvolvimento | `pnpm dev` | Inicia o servidor Next.js em modo desenvolvimento |
| Build | `pnpm build` | Gera o build de produção |
| Produção | `pnpm start` | Inicia o servidor em modo produção (requer build) |
| Lint | `pnpm lint` | Roda o ESLint em todo o projeto |
| Formatar | `pnpm format` | Formata todos os arquivos com Prettier |
| Checar formato | `pnpm format:check` | Verifica se os arquivos estão formatados (usado no CI) |
| Migration | `pnpm db:migrate` | Cria e aplica uma nova migration |
| Generate | `pnpm db:generate` | Regenera o Prisma Client |
| Studio | `pnpm db:studio` | Abre o Prisma Studio na porta 5555 |
| Seed | `pnpm db:seed` | Executa o arquivo `prisma/seed.ts` |
| Reset banco | `pnpm db:reset` | Apaga o banco e reaplica tudo do zero |
| Testes | `pnpm test` | Roda Vitest em modo watch |
| Testes (CI) | `pnpm test --run` | Roda Vitest uma vez e encerra |
| Testes UI | `pnpm test:ui` | Abre a interface visual do Vitest |
| Setup | `pnpm setup` | Instala dependências e cria o `.env` a partir do `.env.example` |

---

## O que você precisa criar ao usar em um projeto real

Ao iniciar um novo projeto a partir deste boilerplate, as seguintes coisas **não estão incluídas** e precisam ser criadas:

### Obrigatório

- [ ] **Models no `prisma/schema.prisma`** — defina as entidades do seu projeto
- [ ] **Migrations** — rode `pnpm db:migrate` após definir os models
- [ ] **`.env`** — preencha com os valores reais do seu ambiente (`cp .env.example .env`)
- [ ] **`prisma/seed.ts`** — crie dados iniciais para desenvolvimento

### Dependendo do projeto

- [ ] **`Dockerfile`** — necessário para deploy em VPS, Railway, Fly.io, etc. Não necessário para Vercel.
- [ ] **Autenticação** — escolha e configure Better Auth, Clerk ou JWT manual
- [ ] **`src/providers/QueryProvider.tsx`** — se usar React Query, crie e adicione ao `layout.tsx`
- [ ] **Variáveis de ambiente adicionais** — adicione no `.env` e `.env.example`
- [ ] **shadcn/ui** — se quiser componentes prontos: `pnpm dlx shadcn@latest init`
- [ ] **Deploy workflow** — adicione um `.github/workflows/deploy.yml` se quiser CD automático

### Dockerfile base (quando necessário)

```dockerfile
FROM node:20-alpine AS base
RUN npm install -g pnpm

FROM base AS deps
WORKDIR /app
COPY package.json pnpm-lock.yaml ./
RUN pnpm install --frozen-lockfile

FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN pnpm build

FROM base AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static
COPY --from=builder /app/public ./public
EXPOSE 3000
CMD ["node", "server.js"]
```

> Para usar o output standalone, adicione `output: "standalone"` no `next.config.ts`.

---

## Como manter o boilerplate atualizado

O boilerplate deve evoluir conforme você aprende e resolve problemas recorrentes. Regra geral: **se você configurou algo chato em um projeto, volta aqui e atualiza**.

### Quando atualizar

- Resolveu um problema de configuração (ESLint, Prisma, Docker, CI)
- Adicionou uma lib que faz sentido em todo projeto (ex: React Query, Zod)
- Encontrou uma estrutura de pasta melhor
- Atualizou uma dependência major e precisou adaptar configs

### Como atualizar

```bash
# Clone o boilerplate diretamente (não via template)
git clone https://github.com/seu-usuario/next-fullstack-boilerplate.git
cd next-fullstack-boilerplate

# Faça as alterações necessárias
# Commit com mensagem descritiva
git add .
git commit -m "chore: update Prisma to v8 and adapt prisma.config.ts"
git push origin main
```

### Convenção de commits no boilerplate

Use prefixos para facilitar o histórico:

| Prefixo | Quando usar |
|---|---|
| `chore:` | Atualização de dependências, configs, scripts |
| `feat:` | Adição de nova lib ou funcionalidade base |
| `fix:` | Correção de configuração quebrada |
| `docs:` | Atualização do README |
| `style:` | Formatação, sem mudança de lógica |
| `refactor:` | Reorganização de estrutura de pastas ou arquivos |

### Atualizando dependências

```bash
# Ver dependências desatualizadas
pnpm outdated

# Atualizar interativamente
pnpm update --interactive --latest
```

> Após atualizar dependências major, sempre rode `pnpm dev`, `pnpm build` e `pnpm test --run` para garantir que nada quebrou.

---

## Convenções de commit

Este boilerplate segue o padrão **Conventional Commits**:

```
tipo(escopo opcional): descrição curta

corpo opcional

rodapé opcional
```

Exemplos:
```
feat(auth): add Better Auth with Prisma adapter
fix(prisma): move DATABASE_URL to prisma.config.ts
chore: update dependencies
docs: update README with auth section
```