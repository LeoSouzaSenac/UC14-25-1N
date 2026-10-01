# Deploy de Backend TypeScript no Render

> **Data de referência:** outubro de 2026  
> Plataformas de hospedagem podem alterar menus, planos e interfaces com o tempo. Caso algum botão esteja em uma posição diferente, consulte a documentação oficial do Render.

# 1. Objetivos

Ao final deste material, você deverá ser capaz de:

- entender o que significa publicar um backend;
- criar uma API simples utilizando Express e TypeScript;
- organizar o projeto utilizando Entity, Service, Controller e Routes;
- utilizar TypeORM;
- configurar o projeto para utilizar MySQL;
- utilizar variáveis de ambiente;
- preparar o projeto para produção;
- testar o build localmente;
- enviar o backend para o GitHub;
- criar um Web Service no Render;
- configurar Build Command e Start Command;
- publicar uma API;
- acessar uma API utilizando uma URL pública;
- atualizar automaticamente o backend utilizando `git push`;
- identificar erros comuns durante o deploy.

---

# 2. O que vamos publicar?

Nos materiais anteriores, um frontend poderia ser publicado em serviços como a Vercel.

Agora o objetivo será publicar o servidor da aplicação.

Durante o desenvolvimento:

```text
MEU COMPUTADOR

Frontend
http://localhost:5173

Backend
http://localhost:3000

Banco MySQL
localhost:3306
```

Depois do deploy, teremos algo semelhante a:

```text
Frontend
Vercel

Backend
Render

Banco
Aiven
```

O fluxo será:

```text
Frontend
   |
   | HTTP
   v
Backend no Render
   |
   | conexão MySQL
   v
Banco no Aiven
```

Neste material trabalharemos apenas com o backend.

O banco no Aiven será configurado posteriormente.

---

# 3. Tecnologias utilizadas

Nosso exemplo utilizará:

```text
Node.js
Express
TypeScript
TypeORM
MySQL
dotenv
cors
```

A estrutura seguirá uma organização semelhante a:

```text
src

config
controllers
entities
routes
services

app.ts
server.ts
```

---

# 4. Criando o projeto

Crie uma pasta:

```text
backend-render
```

Abra essa pasta no VS Code.

No terminal:

```bash
npm init -y
```

Isso criará:

```text
package.json
```

---

# 5. Instalando as dependências

Instale as dependências principais:

```bash
npm install express cors dotenv typeorm reflect-metadata mysql2
```

Agora as dependências utilizadas somente durante o desenvolvimento:

```bash
npm install -D typescript tsx @types/node @types/express @types/cors
```

---

# 6. Para que serve cada biblioteca?

## Express

Responsável pela criação do servidor e das rotas HTTP.

```text
GET
POST
PUT
DELETE
```

## TypeScript

Adiciona tipagem ao JavaScript.

## TypeORM

Object Relational Mapper.

Permite trabalhar com tabelas do banco utilizando classes TypeScript.

## mysql2

Driver utilizado pelo TypeORM para conectar com MySQL.

## dotenv

Carrega variáveis do arquivo:

```text
.env
```

## cors

Controla quais aplicações podem realizar requisições para nossa API.

## reflect-metadata

Biblioteca utilizada pelo sistema de decorators do TypeORM.

---

# 7. Criando o tsconfig.json

Execute:

```bash
npx tsc --init
```

Depois substitua o conteúdo do `tsconfig.json` por:

```json
{
  // Visit https://aka.ms/tsconfig to read more about this file
  "compilerOptions": {
    "module": "nodenext",
    "target": "esnext",
    "strict": true,
     "esModuleInterop": true,
        "experimentalDecorators": true,
        "emitDecoratorMetadata": true,
        "skipLibCheck": true
    },
    "include": [
        "src/**/*"
    ]
}
```

Algumas opções importantes:

```text
rootDir
```

indica onde está nosso código TypeScript.

```text
outDir
```

indica onde será criado o JavaScript compilado.

Neste projeto:

```text
src
```

será transformado em:

```text
dist
```

---

# 8. Estrutura inicial

Crie:

```text
backend-render
│
├── src
│   ├── config
│   ├── controllers
│   ├── entities
│   ├── routes
│   ├── services
│   │
│   ├── app.ts
│   └── server.ts
│
├── .env
├── .gitignore
├── package.json
└── tsconfig.json
```

---

# 9. Criando o .gitignore

Crie:

```text
.gitignore
```

Adicione:

```gitignore
node_modules
dist
.env
```

Nunca envie credenciais do banco para um repositório público.

Principalmente:

```text
senha do banco
usuário
host privado
JWT_SECRET
chaves de API
```

---

# 10. Criando o .env

Durante o desenvolvimento local, crie:

```text
.env
```

Exemplo:

```env
PORT=3000

DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=1234
DB_NAME=crud_render
```

Esses valores são apenas exemplos.

Utilize as informações do seu MySQL local.

Mais tarde essas informações serão substituídas pelas credenciais fornecidas pelo Aiven.

---

# 11. Criando o banco local

No MySQL:

```sql
CREATE DATABASE crud_render;
```

Depois:

```sql
USE crud_render;
```

Não precisamos criar a tabela manualmente neste exemplo.

O TypeORM fará isso durante o desenvolvimento.

---

# 12. Configurando o TypeORM

Crie:

```text
src/config/data-source.ts
```

Conteúdo:

```ts
import "reflect-metadata";
import { DataSource } from "typeorm";
import { User } from "../entities/User";
import * as dotenv from 'dotenv'

dotenv.config()

export const AppDataSource = new DataSource({
    type: "mysql",

    host: process.env.DB_HOST,

    port: Number(process.env.DB_PORT),

    username: process.env.DB_USER,

    password: process.env.DB_PASSWORD,

    database: process.env.DB_NAME,

    entities: [User],

    synchronize: true,

    logging: false
});
```

O `DataSource` representa a conexão utilizada pelo TypeORM.

A configuração não possui:

```ts
host: "localhost"
```

diretamente.

Utilizamos:

```ts
host: process.env.DB_HOST
```

Isso será fundamental durante o deploy.

---

# 13. Por que utilizar variáveis de ambiente?

Imagine escrever:

```ts
password: "MinhaSenhaSuperSecreta123"
```

Depois enviar o projeto para o GitHub.

Qualquer pessoa com acesso ao código poderia encontrar a senha.

Por isso utilizamos:

```ts
password: process.env.DB_PASSWORD
```

Localmente o valor estará no:

```text
.env
```

No Render ele será cadastrado diretamente nas configurações do serviço.

---

# 14. Criando a Entity

Crie:

```text
src/entities/User.ts
```

Conteúdo:

```ts
import {
    Entity,
    PrimaryGeneratedColumn,
    Column
} from "typeorm";

@Entity("users")
export class User {

    @PrimaryGeneratedColumn()
    id!: number;

    @Column('varchar')
    name!: string;

    @Column('varchar',{unique: true
    })
    email!: string;
}
```

representa uma coluna.

---

# 16. Criando o Service

Crie:

```text
src/services/UserService.ts
```

Conteúdo:

```ts
import { AppDataSource } from "../config/data-source";
import { User } from "../entities/User";

export class UserService {

    private userRepository = AppDataSource.getRepository(User);

    async findAll() {
        return await this.userRepository.find();
    }

    async findById(id: number) {

        return await this.userRepository.findOne({
            where: {
                id
            }
        });
    }

    async create(name: string, email: string) {

        const user = this.userRepository.create({
            name,
            email
        });

        return await this.userRepository.save(user);
    }

    async update(
        id: number,
        name: string,
        email: string
    ) {

        const user = await this.findById(id);

        if (!user) {
            return null;
        }

        user.name = name;
        user.email = email;

        return await this.userRepository.save(user);
    }

    async delete(id: number) {

        const user = await this.findById(id);

        if (!user) {
            return false;
        }

        await this.userRepository.remove(user);

        return true;
    }
}
```

---

# 17. Qual é a função do Service?

O Service concentra regras relacionadas à aplicação.

Neste exemplo:

```text
buscar usuários
buscar usuário por ID
cadastrar
alterar
excluir
```

O Controller não precisa conhecer todos os detalhes do banco.

Ele solicita a operação ao Service.

Fluxo:

```text
Controller

Service

TypeORM

Banco
```

---

# 18. Criando o Controller

Crie:

```text
src/controllers/UserController.ts
```

Conteúdo:

```ts
import {
    Request,
    Response
} from "express";

import { UserService } from "../services/UserService";

export class UserController {

    private userService = new UserService();

    findAll = async (
        req: Request,
        res: Response
    ) => {

        try {

            const users =
                await this.userService.findAll();

            return res.json(users);

        } catch (error) {

            console.error(error);

            return res.status(500).json({
                message: "Erro ao buscar usuários"
            });
        }
    };

    findById = async (
        req: Request,
        res: Response
    ) => {

        try {

            const id = Number(req.params.id);

            const user =
                await this.userService.findById(id);

            if (!user) {

                return res.status(404).json({
                    message: "Usuário não encontrado"
                });
            }

            return res.json(user);

        } catch (error) {

            console.error(error);

            return res.status(500).json({
                message: "Erro ao buscar usuário"
            });
        }
    };

    create = async (
        req: Request,
        res: Response
    ) => {

        try {

            const {
                name,
                email
            } = req.body;

            if (!name || !email) {

                return res.status(400).json({
                    message:
                        "Nome e email são obrigatórios"
                });
            }

            const user =
                await this.userService.create(
                    name,
                    email
                );

            return res.status(201).json(user);

        } catch (error) {

            console.error(error);

            return res.status(500).json({
                message: "Erro ao criar usuário"
            });
        }
    };

    update = async (
        req: Request,
        res: Response
    ) => {

        try {

            const id = Number(req.params.id);

            const {
                name,
                email
            } = req.body;

            if (!name || !email) {

                return res.status(400).json({
                    message:
                        "Nome e email são obrigatórios"
                });
            }

            const user =
                await this.userService.update(
                    id,
                    name,
                    email
                );

            if (!user) {

                return res.status(404).json({
                    message: "Usuário não encontrado"
                });
            }

            return res.json(user);

        } catch (error) {

            console.error(error);

            return res.status(500).json({
                message: "Erro ao atualizar usuário"
            });
        }
    };

    delete = async (
        req: Request,
        res: Response
    ) => {

        try {

            const id = Number(req.params.id);

            const deleted =
                await this.userService.delete(id);

            if (!deleted) {

                return res.status(404).json({
                    message: "Usuário não encontrado"
                });
            }

            return res.status(204).send();

        } catch (error) {

            console.error(error);

            return res.status(500).json({
                message: "Erro ao excluir usuário"
            });
        }
    };
}
```

---

# 19. Qual é a função do Controller?

O Controller recebe uma requisição HTTP.

Exemplo:

```text
POST /users
```

Ele recebe:

```json
{
    "name": "Ana",
    "email": "ana@email.com"
}
```

Depois chama:

```text
UserService
```

E devolve uma resposta HTTP.

Fluxo completo:

```text
REQUISIÇÃO

Controller

Service

Repository TypeORM

MySQL

Service

Controller

RESPOSTA
```

---

# 20. Criando as rotas

Crie:

```text
src/routes/user.routes.ts
```

Conteúdo:

```ts
import { Router } from "express";
import { UserController } from "../controllers/UserController";

const router = Router();

const userController =
    new UserController();

router.get(
    "/",
    userController.findAll
);

router.get(
    "/:id",
    userController.findById
);

router.post(
    "/",
    userController.create
);

router.put(
    "/:id",
    userController.update
);

router.delete(
    "/:id",
    userController.delete
);

export default router;
```

---

# 21. Rotas disponíveis

Teremos:

```text
GET /users
```

Lista usuários.

```text
GET /users/:id
```

Busca um usuário.

```text
POST /users
```

Cria um usuário.

```text
PUT /users/:id
```

Atualiza um usuário.

```text
DELETE /users/:id
```

Exclui um usuário.

Isso representa um CRUD.

CRUD significa:

```text
Create
Read
Update
Delete
```

---

# 22. Criando o app.ts

Crie:

```text
src/app.ts
```

Conteúdo:

```ts
import express from "express";
import cors from "cors";

import userRoutes from "./routes/user.routes";

const app = express();

app.use(cors());

app.use(express.json());

app.get("/", (req, res) => {

    return res.json({
        message: "API funcionando"
    });
});

app.get("/health", (req, res) => {

    return res.status(200).json({
        status: "ok"
    });
});

app.use(
    "/users",
    userRoutes
);

export default app;
```

---

# 23. A rota /health

Criamos:

```text
GET /health
```

Ela simplesmente retorna:

```json
{
    "status": "ok"
}
```

Essa rota é útil para verificar se o servidor está respondendo.

Depois do deploy poderemos testar:

```text
https://meu-backend.onrender.com/health
```

---

# 24. Criando o server.ts

Crie:

```text
src/server.ts
```

Conteúdo:

```ts
import "reflect-metadata";
import * as dotenv from 'dotenv'

dotenv.config()

import app from "./app";

import {
    AppDataSource
} from "./config/data-source";

const PORT =
    Number(process.env.PORT) || 3000;

async function startServer() {

    try {

        await AppDataSource.initialize();

        console.log(
            "Banco conectado com sucesso"
        );

        app.listen(
            PORT,
            "0.0.0.0",
            () => {

                console.log(
                    `Servidor executando na porta ${PORT}`
                );
            }
        );

    } catch (error) {

        console.error(
            "Erro ao iniciar servidor:",
            error
        );

        process.exit(1);
    }
}

startServer();
```

---

# 25. Atenção com a porta

Durante o desenvolvimento usamos normalmente:

```text
3000
```

Mas em produção não devemos depender apenas disso.

Por isso:

```ts
const PORT =
    Number(process.env.PORT) || 3000;
```

Significa:

```text
se PORT existir

utilize PORT
```

caso contrário:

```text
utilize 3000
```

No computador:

```text
PORT=3000
```

No Render:

```text
PORT definida pelo ambiente
```

O servidor também utiliza:

```ts
"0.0.0.0"
```

em vez de:

```text
localhost
```

Isso permite que o serviço seja acessado externamente pelo Render.

---

# 26. Configurando os scripts

Abra:

```text
package.json
```

Configure os scripts:

```json
{
    "scripts": {
        "dev": "tsx watch src/server.ts",
        "build": "tsc",
        "start": "node dist/server.js"
    }
}
```

Não apague as dependências que já existem no arquivo.

O `package.json` completo terá uma estrutura semelhante a:

```json
{
    "name": "backend-render",
    "version": "1.0.0",
    "main": "dist/server.js",

    "scripts": {
        "dev": "tsx watch src/server.ts",
        "build": "tsc",
        "start": "node dist/server.js"
    },

    "dependencies": {
        "cors": "...",
        "dotenv": "...",
        "express": "...",
        "mysql2": "...",
        "reflect-metadata": "...",
        "typeorm": "..."
    },

    "devDependencies": {
        "@types/cors": "...",
        "@types/express": "...",
        "@types/node": "...",
        "tsx": "...",
        "typescript": "..."
    }
}
```

As versões serão preenchidas pelo npm.

---

# 27. Desenvolvimento e produção

Durante o desenvolvimento:

```bash
npm run dev
```

O `tsx` executa diretamente os arquivos TypeScript.

Em produção teremos outro processo.

Primeiro:

```bash
npm run build
```

Isso executa:

```text
tsc
```

E transforma:

```text
src
```

em:

```text
dist
```

Depois:

```bash
npm start
```

executa:

```text
dist/server.js
```

---

# 28. Testando localmente

Execute:

```bash
npm run dev
```

Você deverá visualizar algo semelhante a:

```text
Banco conectado com sucesso
Servidor executando na porta 3000
```

Abra:

```text
http://localhost:3000
```

Resposta:

```json
{
    "message": "API funcionando"
}
```

Teste também:

```text
http://localhost:3000/health
```

Resposta:

```json
{
    "status": "ok"
}
```

---

# 29. Testando o CRUD

Você pode utilizar:

```text
Postman
Insomnia
Thunder Client
```

ou qualquer cliente HTTP.

## Criar usuário

```text
POST

http://localhost:3000/users
```

Body JSON:

```json
{
    "name": "Leonardo",
    "email": "leonardo@email.com"
}
```

---

# 30. Listar usuários

```text
GET

http://localhost:3000/users
```

Resposta possível:

```json
[
    {
        "id": 1,
        "name": "Leonardo",
        "email": "leonardo@email.com"
    }
]
```

---

# 31. Buscar usuário

```text
GET

http://localhost:3000/users/1
```

---

# 32. Atualizar usuário

```text
PUT

http://localhost:3000/users/1
```

Body:

```json
{
    "name": "Leonardo Souza",
    "email": "leonardo@email.com"
}
```

---

# 33. Excluir usuário

```text
DELETE

http://localhost:3000/users/1
```

---

# 34. Testando o build antes do deploy

Esse passo é muito importante.

Execute:

```bash
npm run build
```

Se tudo estiver correto, será criada:

```text
dist
```

Estrutura:

```text
backend-render
│
├── dist
│
├── src
│
├── node_modules
│
├── .env
├── .gitignore
├── package.json
├── package-lock.json
└── tsconfig.json
```

Agora teste exatamente a versão que será utilizada em produção:

```bash
npm start
```

Teste novamente:

```text
http://localhost:3000
```

e:

```text
http://localhost:3000/users
```

Se:

```bash
npm run dev
```

funciona, mas:

```bash
npm run build
```

falha, o projeto ainda não está pronto para deploy.

---

# 35. O que vai para o GitHub?

Devemos enviar:

```text
src
package.json
package-lock.json
tsconfig.json
.gitignore
```

Não devemos enviar:

```text
node_modules
.env
dist
```

O servidor de hospedagem instalará as dependências e criará seu próprio build.

---

# 36. Criando o repositório Git

Dentro do projeto:

```bash
git init
```

Depois:

```bash
git add .
```

Commit:

```bash
git commit -m "backend pronto para deploy"
```

Defina:

```bash
git branch -M main
```

---

# 37. Criando o repositório no GitHub

Crie um repositório no GitHub.

Exemplo:

```text
backend-crud-render
```

Depois conecte:

```bash
git remote add origin URL_DO_REPOSITORIO
```

Exemplo:

```bash
git remote add origin https://github.com/usuario/backend-crud-render.git
```

Envie:

```bash
git push -u origin main
```

---

# 38. Conferindo o GitHub

Abra o repositório.

Confira se existem:

```text
src
package.json
package-lock.json
tsconfig.json
```

Confira principalmente se NÃO existe:

```text
.env
```

Se seu `.env` contendo senha apareceu no GitHub, remover o arquivo depois não significa necessariamente que a senha nunca foi exposta.

Nesse caso, altere as credenciais.

---

# 39. Criando a conta no Render

Acesse:

```text
https://render.com
```

Crie uma conta.

Uma opção prática é utilizar sua conta do GitHub.

Isso permite conectar diretamente os repositórios.

---

# 40. Criando um Web Service

Dentro do painel do Render procure:

```text
New
```

Depois:

```text
Web Service
```

Backend Node.js precisa de um processo executando continuamente.

Por isso utilizamos:

```text
Web Service
```

e não:

```text
Static Site
```

---

# 41. Conectando o GitHub

O Render solicitará acesso ao GitHub.

Você poderá liberar:

```text
todos os repositórios
```

ou:

```text
repositórios selecionados
```

Para aula, pode ser interessante selecionar somente o repositório utilizado.

Depois localize:

```text
backend-crud-render
```

e selecione o projeto.

---

# 42. Name

Escolha um nome.

Exemplo:

```text
backend-crud-render
```

Esse nome poderá fazer parte do endereço público.

Exemplo:

```text
https://backend-crud-render.onrender.com
```

Caso o nome já esteja sendo utilizado, poderá ser necessário utilizar outro.

---

# 43. Branch

Selecione:

```text
main
```

Assim o Render acompanhará a branch principal.

---

# 44. Root Directory

Se o repositório contém somente o backend:

```text
meu-backend
│
├── src
├── package.json
└── tsconfig.json
```

deixe o Root Directory vazio.

Mas imagine:

```text
meu-projeto
│
├── frontend
└── backend
```

e dentro de:

```text
backend
```

está o:

```text
package.json
```

Nesse caso configure:

```text
Root Directory

backend
```

Isso informa ao Render onde está o projeto Node.js.

---

# 45. Runtime ou Language

Escolha:

```text
Node
```

O Render executará nossa aplicação em um ambiente Node.js.

---

# 46. Build Command

Configure:

```bash
npm install && npm run build
```

Esse comando fará:

```text
instalar dependências

compilar TypeScript

criar dist
```

Também seria possível utilizar fluxos baseados em instalação limpa das dependências, mas para este primeiro exemplo utilizaremos o comando acima por ser simples de compreender.

---

# 47. Start Command

Configure:

```bash
npm start
```

Lembre que no nosso `package.json`:

```json
"start": "node dist/server.js"
```

Portanto o Render executará:

```text
npm start
```

que executará:

```text
node dist/server.js
```

---

# 48. Fluxo do deploy

Quando fizermos deploy, o Render realizará aproximadamente:

```text
GitHub

clonar projeto

npm install

npm run build

TypeScript vira JavaScript

dist

npm start

servidor iniciado
```

---

# 49. Variáveis de ambiente

Nosso computador possui:

```text
.env
```

Mas esse arquivo não foi enviado ao GitHub.

Então o Render ainda não sabe:

```text
DB_HOST
DB_PORT
DB_USER
DB_PASSWORD
DB_NAME
```

Essas informações precisam ser cadastradas como variáveis de ambiente.

No Render procure pela área de:

```text
Environment
```

ou:

```text
Environment Variables
```

---

# 50. Variáveis do banco

Posteriormente, quando configurarmos o Aiven, teremos valores semelhantes a:

```text
DB_HOST
DB_PORT
DB_USER
DB_PASSWORD
DB_NAME
```

O Aiven fornecerá os valores reais.

No Render cadastraremos:

```text
DB_HOST=...
DB_PORT=...
DB_USER=...
DB_PASSWORD=...
DB_NAME=...
```

Não precisamos criar:

```text
.env
```

dentro do servidor.

O Render disponibiliza essas informações para:

```ts
process.env
```

---

# 51. E a variável PORT?

Nosso código possui:

```ts
const PORT =
    Number(process.env.PORT) || 3000;
```

Em produção, a plataforma fornece uma porta para o serviço.

Portanto não devemos escrever algo como:

```ts
app.listen(3000);
```

e depender obrigatoriamente dela.

Utilizamos:

```ts
process.env.PORT
```

---

# 52. Por que usamos 0.0.0.0?

Nosso servidor possui:

```ts
app.listen(
    PORT,
    "0.0.0.0"
);
```

Localmente é comum pensar apenas em:

```text
localhost
```

Mas em um serviço publicado o servidor precisa aceitar conexões externas encaminhadas pela infraestrutura da hospedagem.

Por isso utilizamos:

```text
0.0.0.0
```

---

# 53. Banco ainda não configurado

Existe um detalhe importante.

Nosso código inicializa primeiro:

```ts
await AppDataSource.initialize();
```

e somente depois inicia o servidor.

Portanto, se o banco não estiver acessível, o servidor não iniciará.

Isso é proposital.

O fluxo será:

```text
1. criar backend

2. testar com MySQL local

3. criar banco no Aiven

4. cadastrar credenciais do Aiven no Render

5. realizar deploy
```

Não tente utilizar no Render:

```env
DB_HOST=localhost
```

O `localhost` do Render representa a própria máquina/container do backend.

Seu MySQL do computador não está lá.

---

# 54. Fazendo o primeiro deploy

Depois de preencher as configurações, selecione:

```text
Create Web Service
```

ou o botão equivalente apresentado na interface.

O Render começará o deploy.

Nos logs você deverá observar etapas semelhantes a:

```text
Cloning repository

Installing dependencies

Running build

Starting service
```

Nosso projeto deverá executar:

```bash
npm install
```

depois:

```bash
npm run build
```

e finalmente:

```bash
npm start
```

---

# 55. Logs

Durante o deploy, acompanhe os logs.

Se tudo estiver correto, nosso próprio código exibirá:

```text
Banco conectado com sucesso
Servidor executando na porta ...
```

Se isso não aparecer, leia os erros anteriores.

Não observe apenas:

```text
Deploy failed
```

Procure o primeiro erro relevante.

---

# 56. URL pública

Depois de publicado, o Render fornecerá uma URL semelhante a:

```text
https://backend-crud-render.onrender.com
```

Teste:

```text
https://backend-crud-render.onrender.com
```

Resposta:

```json
{
    "message": "API funcionando"
}
```

Teste também:

```text
https://backend-crud-render.onrender.com/health
```

Resposta:

```json
{
    "status": "ok"
}
```

---

# 57. Testando o CRUD publicado

Agora não utilizamos:

```text
http://localhost:3000
```

Utilizamos a URL do Render.

## Listar

```text
GET

https://backend-crud-render.onrender.com/users
```

## Criar

```text
POST

https://backend-crud-render.onrender.com/users
```

Body:

```json
{
    "name": "Maria",
    "email": "maria@email.com"
}
```

## Buscar

```text
GET

https://backend-crud-render.onrender.com/users/1
```

## Atualizar

```text
PUT

https://backend-crud-render.onrender.com/users/1
```

## Excluir

```text
DELETE

https://backend-crud-render.onrender.com/users/1
```

---

# 58. Localhost deixou de ser a API

Antes:

```text
http://localhost:3000/users
```

Depois:

```text
https://backend-crud-render.onrender.com/users
```

Esse é um dos principais conceitos do deploy.

Antes:

```text
API existe apenas no meu computador
```

Depois:

```text
API existe em um servidor acessível pela internet
```

---

# 59. Conectando posteriormente com o frontend

Quando tivermos um frontend React, durante o desenvolvimento poderemos ter:

```env
VITE_API_URL=http://localhost:3000
```

Depois do deploy:

```env
VITE_API_URL=https://backend-crud-render.onrender.com
```

No frontend:

```ts
fetch(
    `${import.meta.env.VITE_API_URL}/users`
);
```

Assim não precisamos alterar todas as requisições manualmente.

---

# 60. CORS

Neste exemplo utilizamos:

```ts
app.use(cors());
```

Isso facilita os testes porque permite requisições de diferentes origens.

Em uma aplicação real podemos restringir o frontend permitido.

Exemplo:

```ts
app.use(
    cors({
        origin: "https://meu-frontend.vercel.app"
    })
);
```

Durante o desenvolvimento, pode ser necessário permitir também:

```text
http://localhost:5173
```

---

# 61. Atualizando o backend

Depois que o Render estiver conectado ao GitHub, altere alguma coisa.

Por exemplo:

```ts
app.get("/", (req, res) => {

    return res.json({
        message: "API versão 2"
    });
});
```

Depois:

```bash
git add .
```

```bash
git commit -m "atualizando api"
```

```bash
git push
```

O fluxo será:

```text
alteração

git commit

git push

GitHub

Render

novo build

novo deploy
```

Não é necessário criar outro Web Service.

---

# 62. Deploy automático

Quando o deploy automático estiver habilitado, novos commits enviados para a branch configurada podem gerar novos deploys.

Exemplo:

```text
main

git push

Render detecta alteração

build

deploy
```

Isso permite manter o backend publicado sempre atualizado.

---

# 63. Erro: npm run build falha

Se o Render mostrar um erro durante:

```text
npm run build
```

execute localmente:

```bash
npm run build
```

Corrija os erros de TypeScript.

Depois:

```bash
git add .
git commit -m "corrigindo build"
git push
```

Não tente resolver problemas de compilação somente pelo painel do Render.

---

# 64. Erro: Cannot find module

Exemplo:

```text
Cannot find module 'express'
```

ou:

```text
Cannot find module 'typeorm'
```

Confira:

```text
package.json
```

A biblioteca precisa estar instalada.

Exemplo:

```bash
npm install typeorm
```

Depois envie:

```text
package.json
package-lock.json
```

para o GitHub.

---

# 65. Erro: módulo existente localmente

Pode acontecer de o projeto funcionar porque seu computador possui arquivos dentro de:

```text
node_modules
```

mas a dependência não está declarada corretamente no:

```text
package.json
```

O Render cria um ambiente novo.

Ele não possui os pacotes do seu computador.

---

# 66. Erro: dist/server.js não existe

Se aparecer algo semelhante a:

```text
Cannot find module dist/server.js
```

verifique primeiro se:

```bash
npm run build
```

cria:

```text
dist/server.js
```

Confira também:

```json
"rootDir": "./src",
"outDir": "./dist"
```

e:

```json
"start": "node dist/server.js"
```

Os caminhos precisam ser compatíveis.

---

# 67. Erro: banco não conecta

Pode aparecer algo semelhante a:

```text
Access denied
```

```text
ECONNREFUSED
```

```text
ENOTFOUND
```

Verifique:

```text
DB_HOST
DB_PORT
DB_USER
DB_PASSWORD
DB_NAME
```

Confira também se o banco permite conexão externa.

Quando utilizarmos Aiven, essas informações virão do próprio serviço.

---

# 68. Erro: usuário vazio

Um erro como:

```text
Access denied for user ''@'...'
```

pode indicar que:

```text
DB_USER
```

não existe ou está vazio.

Confira as variáveis de ambiente.

O mesmo vale para:

```text
DB_PASSWORD
DB_HOST
DB_NAME
```

---

# 69. Erro: localhost no banco

Isto funciona no computador:

```env
DB_HOST=localhost
```

Mas não significa que funcionará no Render.

No Render:

```text
localhost
```

representa o ambiente do próprio backend.

Se o banco está no Aiven, devemos utilizar o endereço fornecido pelo Aiven.

Exemplo conceitual:

```env
DB_HOST=mysql-exemplo.aivencloud.com
```

Utilize sempre o valor real fornecido pelo serviço.

---

# 70. Erro: porta fixa

Evite:

```ts
app.listen(3000);
```

Prefira:

```ts
const PORT =
    Number(process.env.PORT) || 3000;
```

e:

```ts
app.listen(
    PORT,
    "0.0.0.0"
);
```

Assim o mesmo projeto funciona:

```text
localmente

e

em produção
```

---

# 71. Erro 502

Um erro:

```text
502 Bad Gateway
```

pode ocorrer quando o serviço não conseguiu iniciar corretamente ou quando o servidor não está disponível na interface e porta esperadas.

Verifique os logs.

Confira principalmente:

```text
PORT
0.0.0.0
npm start
conexão com banco
```

---

# 72. Erro de TypeORM

Se aparecer erro relacionado às Entities, verifique se elas estão registradas:

```ts
entities: [User]
```

Exemplo:

```ts
export const AppDataSource =
    new DataSource({

        type: "mysql",

        // ...

        entities: [
            User
        ]
    });
```

Se uma Entity não estiver configurada corretamente, o TypeORM não conseguirá trabalhar com ela.

---

# 73. synchronize

Neste exemplo utilizamos:

```ts
synchronize: true
```

Isso facilita muito atividades didáticas porque o TypeORM cria ou ajusta a estrutura necessária automaticamente.

Porém, em sistemas reais de produção, alterações automáticas de estrutura podem ser perigosas.

Em projetos profissionais é comum utilizar:

```text
migrations
```

Assim as mudanças no banco são controladas.

Para este primeiro exercício manteremos:

```ts
synchronize: true
```

---

# 74. Health Check

Nossa aplicação possui:

```text
GET /health
```

que responde:

```json
{
    "status": "ok"
}
```

O Render possui suporte a caminhos de Health Check.

Caso configure esse recurso, podemos utilizar:

```text
/health
```

A rota deve ser simples.

Evite colocar operações pesadas nela.

---

# 75. Estrutura final do projeto

Ao final teremos:

```text
backend-render
│
├── src
│   │
│   ├── config
│   │   └── data-source.ts
│   │
│   ├── controllers
│   │   └── UserController.ts
│   │
│   ├── entities
│   │   └── User.ts
│   │
│   ├── routes
│   │   └── user.routes.ts
│   │
│   ├── services
│   │   └── UserService.ts
│   │
│   ├── app.ts
│   └── server.ts
│
├── .env
├── .gitignore
├── package.json
├── package-lock.json
└── tsconfig.json
```

No GitHub:

```text
backend-render
│
├── src
├── .gitignore
├── package.json
├── package-lock.json
└── tsconfig.json
```

Não deverão estar:

```text
node_modules
.env
dist
```

---

# 76. Arquitetura utilizada

Nossa requisição segue:

```text
CLIENTE
   |
   v
ROUTE
   |
   v
CONTROLLER
   |
   v
SERVICE
   |
   v
TYPEORM
   |
   v
MYSQL
```

Exemplo:

```text
POST /users
```

chega em:

```text
user.routes.ts
```

depois:

```text
UserController
```

depois:

```text
UserService
```

depois:

```text
User Repository
```

e finalmente:

```text
MySQL
```

---

# 77. Fluxo local

```text
VS CODE

src

npm run dev

Express

localhost:3000

TypeORM

MySQL local
```

---

# 78. Fluxo de produção

Depois:

```text
GitHub

Render

npm install

npm run build

dist

npm start

Express

URL pública

TypeORM

Aiven
```

---

# 79. Fluxo completo

```text
DESENVOLVIMENTO

VS Code

npm run dev

teste da API

MySQL local

npm run build

npm start

teste da versão compilada

Git

commit

GitHub

Render

npm install

npm run build

npm start

API pública
```

Depois adicionaremos:

```text
Aiven

MySQL público
```

---

# 80. Checklist antes do deploy

Confira:

```text
npm install funciona
```

```text
npm run dev funciona
```

```text
npm run build funciona
```

```text
npm start funciona
```

```text
GET / funciona
```

```text
GET /health funciona
```

```text
CRUD funciona
```

```text
package.json possui build
```

```text
package.json possui start
```

```text
.env está no .gitignore
```

```text
node_modules está no .gitignore
```

```text
dist está no .gitignore
```

```text
package-lock.json foi enviado
```

```text
servidor utiliza process.env.PORT
```

```text
servidor utiliza 0.0.0.0
```

---

# 81. Configuração resumida do Render

Para este projeto:

```text
Service Type

Web Service
```

```text
Language

Node
```

```text
Branch

main
```

Se backend estiver na raiz:

```text
Root Directory

vazio
```

Se estiver dentro de `/backend`:

```text
Root Directory

backend
```

Build:

```bash
npm install && npm run build
```

Start:

```bash
npm start
```

Variáveis:

```text
DB_HOST
DB_PORT
DB_USER
DB_PASSWORD
DB_NAME
```

O `PORT` será lido pelo backend por:

```ts
process.env.PORT
```

---

# 82. Exercício

Crie uma API contendo uma entidade diferente de `User`.

Exemplos:

```text
Product
Book
Student
Game
Task
Movie
```

Ela deverá possuir pelo menos:

```text
id
nome
mais dois campos
```

Exemplo:

```text
Product

id
name
price
stock
```

---

# 83. CRUD obrigatório

Implemente:

```text
GET /products
```

```text
GET /products/:id
```

```text
POST /products
```

```text
PUT /products/:id
```

```text
DELETE /products/:id
```

Organize utilizando:

```text
Entity

Service

Controller

Routes
```

---

# 84. Publicação

Depois:

1. teste com `npm run dev`;
2. teste todas as rotas;
3. execute `npm run build`;
4. execute `npm start`;
5. envie para o GitHub;
6. crie um Web Service no Render;
7. configure o Build Command;
8. configure o Start Command;
9. configure as variáveis de ambiente;
10. realize o deploy;
11. abra a URL pública;
12. teste todas as rotas novamente.

---

# 85. Entrega

Entregue:

```text
link do repositório GitHub
```

e:

```text
link público do backend no Render
```

Exemplo:

```text
Repositório:

https://github.com/usuario/backend-produtos
```

```text
Backend:

https://backend-produtos.onrender.com
```

Teste antes de enviar.

---

# 86. Próxima etapa

Neste material preparamos:

```text
BACKEND

TypeScript
Express
TypeORM
Render
```

Na próxima etapa adicionaremos:

```text
BANCO

MySQL
Aiven
```

Então nosso projeto completo ficará:

```text
FRONTEND

Vercel

        |
        v

BACKEND

Render

        |
        v

BANCO

Aiven
```

O frontend não terá acesso direto ao banco.

O frontend acessará:

```text
Backend
```

E somente o backend acessará:

```text
Banco
```

Esse será o fluxo correto da aplicação.

# 87. Resumo

Antes:

```text
localhost:3000
```

Depois:

```text
https://meu-backend.onrender.com
```

Durante o desenvolvimento:

```bash
npm run dev
```

Antes de publicar:

```bash
npm run build
```

Teste de produção local:

```bash
npm start
```

Envio:

```bash
git add .
git commit -m "preparando deploy"
git push
```

Render:

```text
GitHub

Build

npm install
npm run build

Start

npm start
```

Banco:

```text
variáveis de ambiente
```

Fluxo final:

```text
Código local

Git

GitHub

Render

API pública

Aiven
```

Com isso, o backend deixa de existir somente no computador do desenvolvedor e passa a ser uma API acessível pela internet.
