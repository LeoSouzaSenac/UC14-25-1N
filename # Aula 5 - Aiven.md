# Banco MySQL no Aiven e integração com Render e Vercel

> **Data de referência:** outubro de 2026  
> Plataformas de hospedagem podem alterar menus, planos, limites e interfaces com o tempo. Caso alguma opção esteja em uma posição diferente, consulte a documentação oficial do Aiven, Render ou Vercel.

# 1. Objetivos

Ao final deste material, você deverá ser capaz de:

- entender o que é o Aiven;
- entender qual será o papel do Aiven na nossa aplicação;
- criar um serviço MySQL no Aiven;
- localizar as informações de conexão;
- criar um banco de dados;
- criar um usuário adicional;
- entender a diferença entre banco local e banco em nuvem;
- conectar o MySQL Workbench ao Aiven;
- utilizar SSL/TLS na conexão;
- adaptar o TypeORM para utilizar o banco do Aiven;
- testar o backend local utilizando o banco remoto;
- configurar as variáveis do banco no Render;
- conectar o backend publicado no Render ao Aiven;
- conectar o frontend publicado na Vercel ao backend;
- identificar erros comuns de conexão.

---

# 2. Onde o Aiven entra na nossa aplicação?

Nos materiais anteriores, trabalhamos com:

```text
Frontend
Vercel

Backend
Render
```

Agora precisamos colocar o banco de dados na internet.

Durante o desenvolvimento local, tínhamos algo semelhante a:

```text
Frontend
http://localhost:5173

Backend
http://localhost:3000

MySQL
localhost:3306
```

O problema é que:

```text
localhost
```

significa:

```text
este próprio computador
```

Quando o backend está no Render, ele não consegue acessar o MySQL instalado no computador do aluno utilizando:

```text
localhost:3306
```

Precisamos de um banco acessível pela internet.

Neste material utilizaremos:

```text
Aiven
```

para hospedar o MySQL.

A arquitetura ficará:

```text
Frontend
Vercel
   |
   | HTTPS
   v
Backend
Render
   |
   | conexão MySQL
   v
Banco
Aiven
```

---

# 3. O que é o Aiven?

O **Aiven** é uma plataforma de serviços de dados em nuvem.

Entre os serviços disponíveis estão tecnologias como:

```text
PostgreSQL
MySQL
Kafka
Valkey
ClickHouse
OpenSearch
```

Neste material utilizaremos:

```text
Aiven for MySQL
```

O Aiven será responsável por manter o servidor MySQL disponível na internet.

Não vamos instalar o MySQL manualmente em uma máquina virtual.

A plataforma fornecerá informações como:

```text
Host
Port
User
Password
Database
SSL
```

Nosso backend utilizará esses dados para se conectar.

---

# 4. O que não será hospedado no Aiven?

É importante separar as responsabilidades.

O Aiven será utilizado para:

```text
Banco MySQL
```

O Aiven não será utilizado neste material para hospedar:

```text
React
Express
Node.js
TypeScript
```

Nossa arquitetura será:

| Parte | Serviço |
|---|---|
| Frontend | Vercel |
| Backend | Render |
| Banco MySQL | Aiven |
| Código | GitHub |

---

# 5. Fluxo completo

Quando o usuário utilizar o sistema:

```text
USUÁRIO

   |

   v

FRONTEND
Vercel

   |

   | requisição HTTP/HTTPS

   v

BACKEND
Render

   |

   | consulta SQL através do TypeORM

   v

MYSQL
Aiven
```

O frontend não deve se conectar diretamente ao banco.

Nunca faça:

```text
Vercel
   |
   v
MySQL
```

O correto é:

```text
Vercel
   |
   v
Render
   |
   v
Aiven
```

---

# 6. Por que o frontend não acessa o banco diretamente?

Para conectar ao MySQL precisamos de informações como:

```text
host
porta
usuário
senha
```

Se colocarmos essas informações no frontend, elas serão entregues ao navegador.

Qualquer pessoa poderia inspecionar o JavaScript da aplicação e encontrar essas credenciais.

Por isso:

```text
Frontend
```

conhece apenas:

```text
URL do backend
```

O backend conhece:

```text
credenciais do banco
```

---

# 7. Antes de começar

Precisamos ter:

```text
conta no Aiven
backend TypeScript
TypeORM
mysql2
dotenv
conta no Render
conta na Vercel
```

Nosso backend já possui uma configuração semelhante a:

```ts
host: process.env.DB_HOST,
port: Number(process.env.DB_PORT),
username: process.env.DB_USER,
password: process.env.DB_PASSWORD,
database: process.env.DB_NAME
```

Isso será muito importante.

Não precisamos escrever diretamente no código:

```ts
host: "algum-servidor.aivencloud.com"
```

As informações ficarão em variáveis de ambiente.

---

# 8. Criando uma conta no Aiven

Acesse:

```text
https://aiven.io
```

ou diretamente o console:

```text
https://console.aiven.io
```

Crie uma conta ou faça login.

Dependendo do momento, o Aiven poderá oferecer:

```text
login por email
login com GitHub
login com Google
```

Depois do login, você será levado ao console.

---

# 9. Projeto dentro do Aiven

O Aiven organiza recursos dentro de projetos.

Podemos pensar assim:

```text
Conta Aiven

   |

   v

Projeto

   |

   v

Serviços
```

Um projeto pode possuir, por exemplo:

```text
MySQL
PostgreSQL
Kafka
```

Para nossa aula precisaremos apenas de:

```text
MySQL
```

---

# 10. Criando um projeto

No console do Aiven, crie um novo projeto caso ainda não exista um.

Escolha um nome.

Exemplo:

```text
crud-aula
```

O nome serve para organizar os recursos.

Depois entre no projeto.

---

# 11. Criando o serviço MySQL

Dentro do projeto, procure uma opção semelhante a:

```text
Create service
```

ou:

```text
Create a new service
```

Selecione:

```text
MySQL
```

O Aiven poderá mostrar diferentes planos e configurações.

Escolha uma configuração adequada à atividade.

Os nomes, preços, limites e disponibilidade de planos podem mudar.

Para aula, utilize o menor plano disponível que seja suficiente para os testes.

---

# 12. Escolhendo a região

Dependendo do plano, o Aiven poderá permitir escolher:

```text
Cloud Provider
Region
```

Exemplos de provedores:

```text
AWS
Google Cloud
Azure
```

Sempre que possível, prefira uma região próxima do backend.

Isso pode diminuir a latência.

Para esta atividade, o ponto principal é manter o serviço funcional.

---

# 13. Nome do serviço

Escolha um nome para o serviço.

Exemplo:

```text
mysql-crud-render
```

Esse nome identifica a instância do MySQL dentro do Aiven.

Depois confirme a criação.

---

# 14. Aguarde a criação

O Aiven precisará preparar o serviço.

Durante esse processo, o status poderá indicar algo como:

```text
Rebuilding
Creating
Starting
```

Não tente conectar antes de o serviço estar disponível.

Quando estiver pronto, o console mostrará o serviço como ativo.

---

# 15. Página Overview

Abra o serviço MySQL.

Procure a página:

```text
Overview
```

Uma das áreas mais importantes será:

```text
Connection information
```

É nessa área que encontraremos os dados usados pelo backend.

---

# 16. Connection information

O Aiven apresenta informações semelhantes a:

```text
Host
Port
User
Password
Database
SSL
Service URI
```

Exemplo fictício:

```text
Host

mysql-crud-render-projeto.a.aivencloud.com
```

```text
Port

12345
```

```text
User

avnadmin
```

```text
Password

SENHA_GERADA_PELO_AIVEN
```

```text
Database

defaultdb
```

Os valores do seu projeto serão diferentes.

---

# 17. Não copie os valores deste material

Os exemplos apresentados aqui são fictícios.

Você precisa usar os dados exibidos no seu próprio:

```text
Connection information
```

Principalmente:

```text
Host
Port
User
Password
Database
```

---

# 18. Atenção à porta

Em uma instalação local do MySQL utilizamos normalmente:

```text
3306
```

No Aiven, a porta pode ser diferente.

Por exemplo:

```text
12345
```

ou outro valor fornecido pela plataforma.

Portanto, não faça isto automaticamente:

```env
DB_PORT=3306
```

Copie a porta que aparece no Aiven.

---

# 19. Banco padrão

O Aiven normalmente disponibiliza um banco padrão.

Um nome comum é:

```text
defaultdb
```

É possível utilizar esse banco.

Porém, para organizar melhor nosso projeto, criaremos um banco específico.

---

# 20. Criando um banco de dados

Dentro do serviço MySQL, procure:

```text
Databases
```

Em interfaces atuais do Aiven, essa área pode aparecer dentro da seção de conexão do serviço.

Selecione:

```text
Create database
```

Digite:

```text
crud_render
```

Confirme.

Agora teremos um banco chamado:

```text
crud_render
```

Esse será o valor da variável:

```text
DB_NAME
```

---

# 21. Nosso banco agora está na nuvem

Antes:

```env
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=1234
DB_NAME=crud_render
```

Agora teremos algo semelhante a:

```env
DB_HOST=mysql-crud-render-projeto.a.aivencloud.com
DB_PORT=12345
DB_USER=avnadmin
DB_PASSWORD=SENHA_DO_AIVEN
DB_NAME=crud_render
```

Ainda falta configurar a conexão segura.

---

# 22. Usuário padrão

O Aiven normalmente cria um usuário administrativo para o serviço.

Um nome comum é:

```text
avnadmin
```

Ele pode ser utilizado nos testes iniciais.

As informações estarão em:

```text
Connection information
```

---

# 23. Criando outro usuário

Também podemos criar um usuário específico para a aplicação.

Procure:

```text
Users
```

Depois:

```text
Add service user
```

ou opção equivalente.

Escolha um nome.

Exemplo:

```text
backend_app
```

O Aiven gerará uma senha.

Guarde as informações.

---

# 24. Por que criar um usuário próprio?

Em projetos maiores, é melhor evitar que todas as aplicações utilizem a mesma conta administrativa.

Podemos ter algo como:

```text
avnadmin
```

para administração.

E:

```text
backend_app
```

para a aplicação.

Isso melhora a organização das credenciais.

Para uma primeira atividade, utilizar `avnadmin` é suficiente.

---

# 25. Autenticação do usuário MySQL

Ao criar usuários, o Aiven pode permitir escolher o método de autenticação.

Um método atual comum é:

```text
caching_sha2_password
```

Bibliotecas modernas, como versões atuais do `mysql2`, suportam esse tipo de autenticação.

Se estiver utilizando bibliotecas muito antigas, poderão ocorrer problemas de compatibilidade.

Para nosso projeto, mantenha:

```text
mysql2
```

atualizado.

---

# 26. O que é SSL/TLS?

Nosso backend estará em outro servidor.

A comunicação será:

```text
Render

   |

   | Internet

   v

Aiven
```

Não queremos que dados trafeguem de forma desprotegida.

TLS é o mecanismo utilizado para criptografar essa comunicação.

É comum encontrar também o termo:

```text
SSL
```

Mesmo quando tecnicamente a conexão utiliza versões modernas de TLS.

---

# 27. Certificado CA

O Aiven fornece um certificado de Certificate Authority.

Procure no serviço:

```text
CA Certificate
```

ou uma opção para baixar o certificado.

Baixe o arquivo.

Um nome comum será:

```text
ca.pem
```

Esse certificado permite que o cliente valide a identidade do serviço utilizado na conexão segura.

---

# 28. Não envie ca.pem ao GitHub

Atualize o `.gitignore`.

Exemplo:

```gitignore
node_modules
dist
.env
*.pem
```

Assim arquivos como:

```text
ca.pem
```

não serão enviados por engano.

---

# 29. Testando com MySQL Workbench

Antes de mexer no código, podemos verificar se o banco está acessível.

Abra:

```text
MySQL Workbench
```

Na tela inicial, crie uma nova conexão.

Procure o botão:

```text
+
```

ao lado de:

```text
MySQL Connections
```

---

# 30. Configurando a conexão no Workbench

Preencha:

```text
Connection Name

Aiven CRUD
```

Depois:

```text
Hostname

HOST fornecido pelo Aiven
```

Exemplo fictício:

```text
mysql-crud-render-projeto.a.aivencloud.com
```

Depois:

```text
Port

PORT fornecida pelo Aiven
```

Não presuma `3306`.

Depois:

```text
Username

avnadmin
```

ou o usuário criado.

---

# 31. Senha no Workbench

Você pode clicar em:

```text
Store in Vault
```

ou opção equivalente.

Cole a senha fornecida pelo Aiven.

Não utilize a senha do seu MySQL local.

---

# 32. Configurando SSL no Workbench

Na configuração da conexão, procure a seção:

```text
SSL
```

ou:

```text
SSL/TLS
```

Informe o arquivo:

```text
ca.pem
```

no campo relacionado ao certificado CA.

Utilizar o certificado é recomendado para verificar a conexão segura com o serviço.

---

# 33. Test Connection

Clique em:

```text
Test Connection
```

Se tudo estiver correto, o Workbench deverá informar que a conexão foi realizada.

Se falhar, confira:

```text
Host
Port
User
Password
SSL
```

---

# 34. Abrindo o banco

Depois de conectar, execute:

```sql
SHOW DATABASES;
```

Você deverá encontrar:

```text
crud_render
```

caso tenha criado esse banco.

Depois:

```sql
USE crud_render;
```

---

# 35. Ainda não precisamos criar a tabela manualmente

No nosso backend usamos:

```ts
synchronize: true
```

durante esta atividade.

Quando o backend conectar, o TypeORM poderá criar a tabela da Entity automaticamente.

No nosso exemplo:

```text
users
```

---

# 36. Primeira opção para SSL no backend

Existem diferentes formas de fornecer o certificado ao Node.js.

Uma opção simples localmente é utilizar o arquivo:

```text
ca.pem
```

A estrutura poderia ficar:

```text
backend-render

src
ca.pem
.env
package.json
```

Porém:

```text
ca.pem
```

não deve ser enviado ao GitHub.

Em produção no Render, utilizaremos uma variável de ambiente ou um arquivo secreto.

---

# 37. Preparando a variável DB_CA

Para manter a configuração simples entre computador e Render, podemos criar:

```text
DB_CA
```

Abra o arquivo:

```text
ca.pem
```

O conteúdo será semelhante a:

```text
-----BEGIN CERTIFICATE-----
MIIE...
...
-----END CERTIFICATE-----
```

Precisamos guardar esse conteúdo no ambiente do backend.

---

# 38. DB_CA no arquivo .env

Uma forma é colocar o certificado em uma única variável.

Exemplo:

```env
DB_CA="-----BEGIN CERTIFICATE-----\nMIIE...\n...\n-----END CERTIFICATE-----"
```

Observe:

```text
\n
```

Eles representam as quebras de linha do certificado.

Não use literalmente o certificado fictício deste exemplo.

Copie o seu `ca.pem`.

---

# 39. Nosso .env local usando Aiven

Antes, tínhamos:

```env
PORT=3000

DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=1234
DB_NAME=crud_render
```

Agora ficará semelhante a:

```env
PORT=3000

DB_HOST=mysql-crud-render-projeto.a.aivencloud.com
DB_PORT=12345
DB_USER=avnadmin
DB_PASSWORD=SUA_SENHA_REAL
DB_NAME=crud_render

DB_CA="-----BEGIN CERTIFICATE-----\n...\n-----END CERTIFICATE-----"
```

---

# 40. Nunca envie esse .env ao GitHub

Nosso `.gitignore` deve continuar contendo:

```gitignore
.env
```

O `.env` possui:

```text
senha do banco
host
usuário
certificado
outros segredos
```

Ele deve permanecer fora do repositório.

---

# 41. Criando um .env.example

Podemos criar:

```text
.env.example
```

Conteúdo:

```env
PORT=3000

DB_HOST=
DB_PORT=
DB_USER=
DB_PASSWORD=
DB_NAME=
DB_CA=
```

Esse arquivo pode ir para o GitHub porque não contém os valores reais.

---

# 42. Adaptando o TypeORM

No material do Render tínhamos:

```text
src/config/data-source.ts
```

Agora precisamos acrescentar a configuração de SSL.

Exemplo:

```ts
import "reflect-metadata";
import { DataSource } from "typeorm";
import { User } from "../entities/User";
import * as dotenv from "dotenv";

dotenv.config();

export const AppDataSource = new DataSource({
    type: "mysql",

    host: process.env.DB_HOST,

    port: Number(process.env.DB_PORT),

    username: process.env.DB_USER,

    password: process.env.DB_PASSWORD,

    database: process.env.DB_NAME,

    entities: [User],

    synchronize: true,

    logging: false,

    ssl: process.env.DB_CA
        ? {
              ca: process.env.DB_CA.replace(/\\n/g, "\n"),
              rejectUnauthorized: true
          }
        : undefined
});
```

---

# 43. O que mudou?

Antes:

```ts
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

Agora acrescentamos:

```ts
ssl: process.env.DB_CA
    ? {
          ca: process.env.DB_CA.replace(/\\n/g, "\n"),
          rejectUnauthorized: true
      }
    : undefined
```

---

# 44. Por que usamos replace?

Dentro da variável temos:

```text
\n
```

como texto.

O Node.js precisa transformar isso em uma quebra de linha real.

Por isso:

```ts
replace(/\\n/g, "\n")
```

transforma:

```text
\n
```

em:

```text
quebra de linha
```

Isso reconstrói o certificado no formato esperado.

---

# 45. O que significa rejectUnauthorized?

Utilizamos:

```ts
rejectUnauthorized: true
```

Isso faz com que o cliente valide o certificado recebido.

Não utilize como solução automática:

```ts
rejectUnauthorized: false
```

apenas para fazer um erro desaparecer.

Desabilitar a validação reduz a segurança da conexão.

---

# 46. Testando o backend local com o Aiven

Agora o backend ainda está no nosso computador.

Mas o banco já está na internet.

O fluxo será:

```text
Backend local
localhost:3000

   |

   v

Aiven MySQL
```

Execute:

```bash
npm run dev
```

---

# 47. Resultado esperado

Se a conexão funcionar, veremos algo semelhante a:

```text
Banco conectado com sucesso
Servidor executando na porta 3000
```

Nosso `server.ts` já faz:

```ts
await AppDataSource.initialize();
```

Portanto, se a conexão com o banco falhar, o servidor deverá apresentar o erro no terminal.

---

# 48. Testando a rota de health

Abra:

```text
http://localhost:3000/health
```

Resposta:

```json
{
    "status": "ok"
}
```

Isso prova que a API está respondendo.

Mas ainda precisamos testar uma rota que use o banco.

---

# 49. Testando uma operação real

Utilize:

```text
POST

http://localhost:3000/users
```

Body:

```json
{
    "name": "Ana",
    "email": "ana@email.com"
}
```

Se funcionar, o backend gravará o usuário no MySQL do Aiven.

---

# 50. Conferindo pelo Workbench

Abra a conexão do Aiven no MySQL Workbench.

Selecione:

```sql
USE crud_render;
```

Depois:

```sql
SHOW TABLES;
```

Você deverá encontrar:

```text
users
```

Depois:

```sql
SELECT * FROM users;
```

O usuário cadastrado pela API deverá aparecer.

---

# 51. O fluxo já funciona

Neste momento temos:

```text
POST /users
   |
   v
Backend local
   |
   v
TypeORM
   |
   v
Aiven
```

Ainda não publicamos o backend.

O próximo passo será conectar:

```text
Render
```

ao mesmo banco.

---

# 52. O backend no Render não lê nosso .env local

Este ponto é importante.

O arquivo:

```text
.env
```

está somente no nosso computador.

Ele não é enviado para o GitHub.

Portanto o Render não receberá automaticamente:

```text
DB_HOST
DB_PORT
DB_USER
DB_PASSWORD
DB_NAME
DB_CA
```

Precisamos cadastrar essas variáveis no Render.

---

# 53. Abrindo o serviço no Render

Acesse:

```text
https://dashboard.render.com
```

Abra o Web Service do backend criado no material anterior.

Procure:

```text
Environment
```

---

# 54. Variáveis do Aiven no Render

Cadastre:

```text
DB_HOST
```

Valor:

```text
HOST fornecido pelo Aiven
```

Depois:

```text
DB_PORT
```

Valor:

```text
PORT fornecida pelo Aiven
```

Depois:

```text
DB_USER
```

Depois:

```text
DB_PASSWORD
```

Depois:

```text
DB_NAME
```

---

# 55. Configurando DB_CA no Render

Também crie:

```text
DB_CA
```

Cole o certificado CA.

É possível armazenar valores multilinha nas variáveis do Render.

Uma alternativa é utilizar a mesma representação que usamos no `.env`:

```text
-----BEGIN CERTIFICATE-----\n...\n-----END CERTIFICATE-----
```

Nosso código possui:

```ts
replace(/\\n/g, "\n")
```

para reconstruir as quebras.

---

# 56. Não cadastre PORT manualmente sem necessidade

Nosso código utiliza:

```ts
const PORT =
    Number(process.env.PORT) || 3000;
```

O Render fornece a variável:

```text
PORT
```

para Web Services.

Portanto não precisamos definir manualmente:

```text
PORT=3000
```

no painel apenas para o deploy funcionar.

---

# 57. Conferindo todas as variáveis no Render

Teremos algo semelhante a:

```text
DB_HOST
mysql-crud-render-projeto.a.aivencloud.com
```

```text
DB_PORT
12345
```

```text
DB_USER
avnadmin
```

```text
DB_PASSWORD
********
```

```text
DB_NAME
crud_render
```

```text
DB_CA
-----BEGIN CERTIFICATE-----...
```

Os valores reais serão os seus.

---

# 58. Salvando as variáveis

Salve as alterações.

Dependendo da interface atual, o Render poderá:

```text
iniciar novo deploy
```

ou permitir que você:

```text
salve e faça deploy depois
```

Realize um novo deploy para garantir que a aplicação utilize as novas variáveis.

---

# 59. Observando os logs

Abra:

```text
Logs
```

ou a área de deploy do serviço.

Procure:

```text
Banco conectado com sucesso
```

Depois:

```text
Servidor executando na porta ...
```

Se aparecer um erro antes disso, leia a mensagem completa.

---

# 60. Testando Render + Aiven

Suponha que nosso backend esteja em:

```text
https://backend-crud-render.onrender.com
```

Teste:

```text
https://backend-crud-render.onrender.com/health
```

Resposta:

```json
{
    "status": "ok"
}
```

Agora teste:

```text
GET

https://backend-crud-render.onrender.com/users
```

---

# 61. Criando um usuário no backend publicado

Utilize Postman, Insomnia ou Thunder Client.

```text
POST

https://backend-crud-render.onrender.com/users
```

Body:

```json
{
    "name": "Carlos",
    "email": "carlos@email.com"
}
```

Se o cadastro funcionar, temos:

```text
Render
   |
   v
Aiven
```

funcionando corretamente.

---

# 62. Conferindo no banco

No Workbench:

```sql
USE crud_render;
```

Depois:

```sql
SELECT * FROM users;
```

O registro criado pelo Render deverá aparecer.

Isso demonstra que:

```text
backend publicado
```

está utilizando:

```text
banco publicado
```

---

# 63. Agora falta o frontend

Neste momento temos:

```text
Render
   |
   v
Aiven
```

Agora precisamos conectar:

```text
Vercel
```

ao Render.

O frontend não receberá nenhuma credencial do Aiven.

Ele precisa apenas da URL da API.

---

# 64. Variável do frontend

Em um projeto React com Vite, podemos utilizar:

```env
VITE_API_URL
```

Localmente:

```env
VITE_API_URL=http://localhost:3000
```

Em produção:

```env
VITE_API_URL=https://backend-crud-render.onrender.com
```

---

# 65. Utilizando VITE_API_URL

Com `fetch`:

```ts
const apiUrl = import.meta.env.VITE_API_URL;

const response = await fetch(
    `${apiUrl}/users`
);
```

Ou:

```ts
const response = await fetch(
    `${import.meta.env.VITE_API_URL}/users`
);
```

---

# 66. Exemplo com Axios

Instale:

```bash
npm install axios
```

Crie uma instância:

```ts
import axios from "axios";

export const api = axios.create({
    baseURL: import.meta.env.VITE_API_URL
});
```

Depois:

```ts
const response = await api.get("/users");
```

---

# 67. Configurando na Vercel

Abra o projeto na Vercel.

Procure:

```text
Settings
```

Depois:

```text
Environment Variables
```

Crie:

```text
VITE_API_URL
```

Valor:

```text
https://backend-crud-render.onrender.com
```

---

# 68. A Vercel não recebe credenciais do Aiven

Na Vercel não coloque:

```text
DB_HOST
DB_PORT
DB_USER
DB_PASSWORD
DB_NAME
DB_CA
```

Também não faça:

```text
VITE_DB_PASSWORD
```

ou:

```text
VITE_DATABASE_URL
```

Variáveis `VITE_` utilizadas pelo frontend podem aparecer no código entregue ao navegador.

---

# 69. Onde fica cada segredo?

## Aiven

Possui:

```text
banco
usuários
senhas
certificados
```

## Render

Recebe:

```text
DB_HOST
DB_PORT
DB_USER
DB_PASSWORD
DB_NAME
DB_CA
```

## Vercel

Recebe:

```text
VITE_API_URL
```

## GitHub

Não recebe:

```text
.env
senha
certificado
```

---

# 70. CORS

Agora teremos origens diferentes.

Frontend:

```text
https://meu-frontend.vercel.app
```

Backend:

```text
https://backend-crud-render.onrender.com
```

Por isso o backend precisa permitir a origem do frontend.

No exemplo mais simples do material anterior tínhamos:

```ts
app.use(cors());
```

Isso libera requisições de diversas origens.

Para uma configuração mais restrita em produção, podemos utilizar uma variável.

---

# 71. FRONTEND_URL

No Render, crie:

```text
FRONTEND_URL
```

Valor:

```text
https://meu-frontend.vercel.app
```

Depois altere o backend:

```ts
app.use(
    cors({
        origin: process.env.FRONTEND_URL
    })
);
```

---

# 72. Permitindo localhost e Vercel

Durante o desenvolvimento podemos querer permitir:

```text
http://localhost:5173
```

e em produção:

```text
https://meu-frontend.vercel.app
```

Exemplo:

```ts
const allowedOrigins = [
    "http://localhost:5173",
    process.env.FRONTEND_URL
].filter(Boolean) as string[];

app.use(
    cors({
        origin: allowedOrigins
    })
);
```

---

# 73. Se o sistema utiliza cookies

Se a autenticação utiliza cookie:

```ts
app.use(
    cors({
        origin: process.env.FRONTEND_URL,
        credentials: true
    })
);
```

No frontend com Axios:

```ts
export const api = axios.create({
    baseURL: import.meta.env.VITE_API_URL,
    withCredentials: true
});
```

Em produção HTTPS, cookies de autenticação entre origens diferentes normalmente também precisam de uma configuração apropriada no backend.

Exemplo:

```ts
res.cookie("token", token, {
    httpOnly: true,
    secure: true,
    sameSite: "none"
});
```

A configuração correta depende da arquitetura de autenticação utilizada pelo projeto.

---

# 74. Arquitetura final

Agora teremos:

```text
USUÁRIO
   |
   v
VERCEL
React + Vite
   |
   | VITE_API_URL
   v
RENDER
Node + Express + TypeScript
   |
   | DB_HOST
   | DB_PORT
   | DB_USER
   | DB_PASSWORD
   | DB_NAME
   | DB_CA
   v
AIVEN
MySQL
```

---

# 75. Quem conhece quem?

## Vercel conhece

```text
Render
```

através de:

```text
VITE_API_URL
```

## Render conhece

```text
Aiven
```

através de:

```text
DB_HOST
DB_PORT
DB_USER
DB_PASSWORD
DB_NAME
DB_CA
```

## Aiven não precisa conhecer a Vercel

O frontend nunca acessa o banco diretamente.

---

# 76. Teste completo

Agora vamos testar tudo.

Abra:

```text
https://meu-frontend.vercel.app
```

Faça uma ação que crie um usuário.

O fluxo será:

```text
Vercel
   |
   v
POST /users
   |
   v
Render
   |
   v
UserController
   |
   v
UserService
   |
   v
TypeORM
   |
   v
Aiven
```

---

# 77. Conferindo o resultado

Depois de cadastrar pelo frontend:

No Workbench:

```sql
SELECT * FROM users;
```

O registro deverá aparecer.

Se isso acontecer, a aplicação inteira está integrada.

---

# 78. O que acontece depois de um git push?

Se GitHub, Render e Vercel estiverem conectados aos respectivos repositórios:

```text
alteração
   |
   v
git add .
   |
   v
git commit
   |
   v
git push
```

O Render poderá gerar um novo deploy do backend.

A Vercel poderá gerar um novo deploy do frontend.

O banco no Aiven continua existente.

---

# 79. O banco não é recriado a cada deploy

Quando fazemos novo deploy no Render:

```text
backend antigo
   |
   v
nova versão do backend
```

o banco do Aiven continua separado.

Isso significa que os dados não devem desaparecer apenas porque publicamos uma nova versão do backend.

---

# 80. Cuidado com synchronize: true

Durante a aprendizagem usamos:

```ts
synchronize: true
```

Isso facilita o projeto porque o TypeORM sincroniza a estrutura das Entities com o banco.

Porém, em aplicações reais de produção, é mais seguro trabalhar com:

```ts
synchronize: false
```

e utilizar:

```text
migrations
```

Alterações de schema em produção devem ser controladas.

---

# 81. Erro: Access denied for user

Exemplo:

```text
Access denied for user
```

Confira:

```text
DB_USER
DB_PASSWORD
```

Verifique também se não existem espaços extras ao copiar a senha.

Confirme se o usuário ainda existe no Aiven.

---

# 82. Erro: connect ECONNREFUSED 127.0.0.1:3306

Se aparecer algo como:

```text
ECONNREFUSED 127.0.0.1:3306
```

o backend provavelmente ainda está tentando utilizar o MySQL local.

Confira:

```text
DB_HOST
```

Se estiver:

```text
localhost
```

ou:

```text
127.0.0.1
```

o Aiven não está sendo utilizado.

---

# 83. Erro: porta incorreta

Se você colocou:

```text
3306
```

sem verificar o Aiven, volte para:

```text
Connection information
```

Copie a porta exibida.

Exemplo:

```env
DB_PORT=12345
```

O número real dependerá do seu serviço.

---

# 84. Erro: Unknown database

Exemplo:

```text
Unknown database 'crud_render'
```

Verifique se o banco foi realmente criado no Aiven.

No Workbench:

```sql
SHOW DATABASES;
```

Confira o nome.

Também confira:

```text
DB_NAME
```

---

# 85. Erro relacionado a certificado

Erros de SSL/TLS podem acontecer quando:

```text
DB_CA está vazia
certificado está incompleto
\n não foi convertido
certificado incorreto foi utilizado
```

Confira o código:

```ts
ca: process.env.DB_CA.replace(/\\n/g, "\n")
```

E:

```ts
rejectUnauthorized: true
```

---

# 86. Erro: self-signed certificate

Uma mensagem relacionada a:

```text
self-signed certificate
```

pode indicar que o cliente não está utilizando corretamente o certificado CA do projeto.

Baixe novamente:

```text
CA Certificate
```

no Aiven.

Depois confira a configuração.

---

# 87. Não resolva tudo desligando a segurança

Ao encontrar erro de certificado, pode ser tentador fazer:

```ts
rejectUnauthorized: false
```

Isso pode esconder o problema, mas também desativa uma verificação importante.

O correto é configurar o CA corretamente.

---

# 88. Erro: backend funciona localmente, mas não no Render

Se:

```text
localhost:3000
```

funciona usando o Aiven, mas:

```text
onrender.com
```

não funciona, confira no Render:

```text
DB_HOST
DB_PORT
DB_USER
DB_PASSWORD
DB_NAME
DB_CA
```

Lembre:

```text
Render não lê o .env do seu computador
```

---

# 89. Erro: health funciona, mas /users falha

Se:

```text
/health
```

funciona, mas:

```text
/users
```

falha, isso pode indicar que:

```text
servidor HTTP está funcionando
```

mas:

```text
operação de banco está falhando
```

Verifique os logs do Render.

Confira a conexão do TypeORM.

---

# 90. Erro: tabela não existe

Exemplo:

```text
Table 'crud_render.users' doesn't exist
```

Durante esta atividade, confira se:

```ts
synchronize: true
```

está ativo.

Também verifique se a Entity está registrada:

```ts
entities: [User]
```

Em produção real com migrations, a solução seria executar as migrations adequadas.

---

# 91. Erro: Vercel continua chamando localhost

Procure no frontend:

```text
localhost:3000
```

Exemplo incorreto:

```ts
fetch("http://localhost:3000/users")
```

Correto:

```ts
fetch(
    `${import.meta.env.VITE_API_URL}/users`
)
```

Na Vercel configure:

```text
VITE_API_URL
```

com a URL do Render.

---

# 92. Erro de CORS

Exemplo:

```text
blocked by CORS policy
```

Confira a URL real da Vercel.

Exemplo:

```text
https://meu-frontend.vercel.app
```

Depois confira:

```text
FRONTEND_URL
```

no Render.

As URLs precisam corresponder corretamente.

---

# 93. Atenção à barra final

Imagine:

```env
VITE_API_URL=https://backend.onrender.com/
```

e no código:

```ts
`${import.meta.env.VITE_API_URL}/users`
```

Isso poderá gerar:

```text
https://backend.onrender.com//users
```

Normalmente é mais simples cadastrar:

```env
VITE_API_URL=https://backend.onrender.com
```

sem a barra final.

---

# 94. Não coloque a URL do Aiven na Vercel

Isto está errado:

```env
VITE_API_URL=mysql-crud.aivencloud.com
```

`VITE_API_URL` deve apontar para:

```text
backend HTTP
```

Exemplo:

```env
VITE_API_URL=https://backend-crud-render.onrender.com
```

O Aiven é acessado pelo backend através do protocolo do MySQL.

---

# 95. Não confunda Service URI com URL de API

O Aiven poderá mostrar uma:

```text
Service URI
```

Ela representa informações de conexão do banco.

Ela não é uma rota HTTP do seu sistema.

Portanto isso:

```text
mysql://...
```

não substitui:

```text
https://backend.onrender.com
```

---

# 96. Posso usar uma única DATABASE_URL?

Algumas aplicações preferem guardar uma string completa de conexão.

Exemplo conceitual:

```text
mysql://usuario:senha@host:porta/banco
```

O TypeORM suporta diferentes formas de configuração.

Porém, neste material continuaremos utilizando:

```text
DB_HOST
DB_PORT
DB_USER
DB_PASSWORD
DB_NAME
```

porque isso facilita visualizar cada parte da conexão.

---

# 97. Posso apagar o MySQL local?

Não é necessário.

Você pode continuar utilizando:

```text
MySQL local
```

para desenvolvimento.

E:

```text
Aiven
```

para ambiente remoto.

Por exemplo:

## Desenvolvimento local

```env
DB_HOST=localhost
DB_PORT=3306
```

## Ambiente remoto

```env
DB_HOST=HOST_DO_AIVEN
DB_PORT=PORT_DO_AIVEN
```

Isso depende da estratégia do projeto.

---

# 98. Mas como testar exatamente o ambiente remoto?

Uma forma prática é temporariamente utilizar no `.env` local:

```text
credenciais do Aiven
```

Assim o backend local acessa o mesmo banco remoto.

Depois, no Render, as mesmas credenciais são cadastradas nas variáveis do serviço.

---

# 99. Cuidados com um banco compartilhado

Se vários alunos utilizarem o mesmo banco:

```text
todos estarão alterando os mesmos dados
```

Para atividades individuais, prefira:

```text
um banco por projeto
```

ou uma separação definida pelo professor.

Evite utilizar o banco de produção para testes destrutivos.

---

# 100. Segurança das credenciais

Nunca publique:

```text
DB_PASSWORD
DB_CA
JWT_SECRET
```

em:

```text
GitHub
README
captura de tela pública
frontend
VITE_*
```

Se uma senha do banco for exposta, considere a credencial comprometida.

Troque a senha.

---

# 101. Se o .env foi enviado ao GitHub

Apenas adicionar:

```text
.env
```

ao `.gitignore` depois não apaga automaticamente a informação do histórico.

A primeira medida deve ser:

```text
trocar a senha exposta
```

Depois remova o arquivo do rastreamento e revise o histórico quando necessário.

---

# 102. Checklist do Aiven

Antes de ir para o Render, confirme:

```text
[ ] Conta criada
[ ] Projeto criado
[ ] Serviço MySQL criado
[ ] Serviço está ativo
[ ] Banco crud_render criado
[ ] Host copiado
[ ] Porta copiada
[ ] Usuário copiado
[ ] Senha copiada
[ ] Certificado CA baixado
[ ] Workbench conectado
[ ] Backend local conectado ao Aiven
[ ] POST /users funciona
[ ] Dados aparecem no banco
```

---

# 103. Checklist do Render

Confira:

```text
[ ] Backend publicado
[ ] DB_HOST configurado
[ ] DB_PORT configurado
[ ] DB_USER configurado
[ ] DB_PASSWORD configurado
[ ] DB_NAME configurado
[ ] DB_CA configurado
[ ] Novo deploy realizado
[ ] Logs mostram conexão com banco
[ ] /health funciona
[ ] GET /users funciona
[ ] POST /users funciona
```

---

# 104. Checklist da Vercel

Confira:

```text
[ ] Frontend publicado
[ ] VITE_API_URL configurada
[ ] VITE_API_URL aponta para o Render
[ ] Novo deploy realizado após alterar variável
[ ] Frontend carrega
[ ] Requisições chegam ao Render
[ ] CORS está correto
[ ] Dados chegam ao Aiven
```

---

# 105. Checklist de segurança

Confira:

```text
[ ] .env está no .gitignore
[ ] *.pem está no .gitignore
[ ] senha não está no código
[ ] senha não está no frontend
[ ] DB_CA não está no frontend
[ ] SSL/TLS está configurado
[ ] certificado é validado
[ ] credenciais expostas foram trocadas
```

---

# 106. Resumo das variáveis

## Backend local

```env
PORT=3000

DB_HOST=
DB_PORT=
DB_USER=
DB_PASSWORD=
DB_NAME=
DB_CA=

FRONTEND_URL=http://localhost:5173
```

## Render

```text
DB_HOST
DB_PORT
DB_USER
DB_PASSWORD
DB_NAME
DB_CA
FRONTEND_URL
```

## Vercel

```text
VITE_API_URL
```

---

# 107. Resumo final

Nossa aplicação começou assim:

```text
Frontend
localhost:5173

Backend
localhost:3000

MySQL
localhost:3306
```

Depois dos três deploys:

```text
Frontend
Vercel

   |

   v

Backend
Render

   |

   v

MySQL
Aiven
```

O frontend utiliza:

```text
VITE_API_URL
```

para encontrar o backend.

O backend utiliza:

```text
DB_HOST
DB_PORT
DB_USER
DB_PASSWORD
DB_NAME
DB_CA
```

para encontrar o banco.

O resultado final é:

```text
Internet
   |
   v
Vercel
   |
   v
Render
   |
   v
Aiven
```

---

# 108. Arquitetura final completa

```text
+----------------------------------+
|             USUÁRIO              |
|            Navegador             |
+----------------+-----------------+
                 |
                 | HTTPS
                 v
+----------------------------------+
|             VERCEL               |
|          React + Vite            |
|                                  |
| VITE_API_URL                     |
| https://backend.onrender.com     |
+----------------+-----------------+
                 |
                 | HTTPS
                 v
+----------------------------------+
|             RENDER               |
| Express + TypeScript + TypeORM   |
|                                  |
| DB_HOST                          |
| DB_PORT                          |
| DB_USER                          |
| DB_PASSWORD                      |
| DB_NAME                          |
| DB_CA                            |
+----------------+-----------------+
                 |
                 | MySQL + TLS
                 v
+----------------------------------+
|              AIVEN               |
|              MySQL               |
|                                  |
| Banco: crud_render               |
| Tabela: users                    |
+----------------------------------+
```

---

# 109. Fontes oficiais

Aiven:

```text
https://aiven.io/docs/products/mysql
```

Conexão com MySQL Workbench:

```text
https://aiven.io/docs/products/mysql/howto/connect-from-mysql-workbench
```

TLS/SSL no Aiven:

```text
https://aiven.io/docs/platform/concepts/tls-ssl-certificates
```

Render:

```text
https://render.com/docs/web-services
```

Variáveis de ambiente no Render:

```text
https://render.com/docs/configure-environment-variables
```

Vercel:

```text
https://vercel.com/docs
```

TypeORM:

```text
https://typeorm.io
```

MySQL2:

```text
https://sidorares.github.io/node-mysql2/
```
