
npm init
npm install typescript@6.0.3 tsx @types/node --save-dev
npx tsc --init
npm install prisma@6.19 @types/node --save-dev
npm install @prisma/client@6.19 dotenv

Em tsconfig.json
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

Em package.json
{
  "type": "module"
}
npx prisma
npx prisma init --datasource-provider mongodb --output ../generated/prisma

Em prisma.config.ts
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

Em prisma/schema.prisma
Apague o   output   = "../generated/prisma"

Em .env 
DATABASE_URL="mongodb+srv://nomeDoProjetoMongoDb:senha@cluster0.w2xad.mongodb.net/nomeDoBanco"

npx prisma db push(ou pull se quizer puchar um pronto)

Em schema.primsa 
generator client {
  provider = "prisma-client-js"
  //output   = "../generated/prisma"
} 
npm i express cors && npm i -D @types/express @types/cors
