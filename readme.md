# 🚀 Deploy Back-End Prisma & MongoDB

Guia passo a passo para inicialização, configuração e implantação da API utilizando Node.js, TypeScript, Express, CORS e o Prisma ORM integrado ao MongoDB.

---

## 🛠️ Instalação e Inicialização

Execute os comandos abaixo na ordem indicada para configurar o ambiente do projeto:

```bash
# 1. Inicializa o gerenciador de pacotes Node
npm init -y

# 2. Instala o TypeScript (v6.0.3), executor tsx e tipos globais do Node
npm install typescript@6.0.3 tsx @types/node --save-dev

# 3. Cria e inicializa o arquivo de configuração do TypeScript
npx tsc --init

# 4. Instala a CLI do Prisma (v6.19) como dependência de desenvolvimento
npm install prisma@6.19 @types/node --save-dev

# 5. Instala o cliente do Prisma (v6.19) e o gerenciador de variáveis de ambiente
npm install @prisma/client@6.19 dotenv

# 6. Instala o Express e o CORS com suas respectivas tipagens TypeScript
npm i express cors && npm i -D @types/express @types/cors
```

---

## ⚙️ Arquivos de Configuração

Configure os arquivos principais na raiz do projeto com as estruturas abaixo:

### 📄 `package.json`
Certifique-se de adicionar a propriedade para permitir módulos ECMAScript:
```json
{
  "type": "module"
}
```

### 📄 `tsconfig.json`
Ajuste as opções do compilador TypeScript:
```json
{
  "compilerOptions": {
    "module": "ESNext",
    "moduleResolution": "bundler",
    "target": "ES2023",
    "strict": true,
    "esModuleInterop": true,
    "ignoreDeprecations": "6.0"
  }
}
```

### 📄 `prisma.config.ts`
Crie este arquivo para centralizar as configurações do Prisma:
```typescript
import "dotenv/config"; 
import { defineConfig, env } from "prisma/config";

export default defineConfig({
  schema: "prisma/schema.prisma",
  migrations: {
    path: "prisma/migrations",
  },
  engine: "classic",
  datasource: {
    url: env("DATABASE_URL"),
  },
});
```

---

## 🗄️ Configuração do Prisma & Banco de Dados

Siga estas etapas específicas para integrar com o MongoDB:

### 1. Inicializar a estrutura do Prisma
```bash
npx prisma init --datasource-provider mongodb --output ../generated/prisma
```

### 2. Configurar Variáveis de Ambiente (`.env`)
Crie o arquivo `.env` na raiz e insira sua string de conexão:
```env
DATABASE_URL="mongodb+srv://nomeDoProjetoMongoDb:senha@cluster0.w2xad.mongodb.net/nomeDoBanco"
```

### 3. Ajustar o arquivo `prisma/schema.prisma`
Abra o arquivo gerado e garanta que o bloco `generator` não possua um output customizado:
```prisma
generator client {
  provider = "prisma-client-js"
  // Certifique-se de apagar ou comentar a linha abaixo se ela existir:
  // output   = "../generated/prisma"
}

datasource db {
  provider = "mongodb"
  url      = env("DATABASE_URL")
}
```

### 4. Sincronizar o Banco de Dados
Para enviar seus schemas locais ou trazer um banco existente, execute:
```bash
# Para enviar alterações locais ao MongoDB
npx prisma db push

# Para puxar tabelas de um banco de dados já existente
npx prisma db pull
```
