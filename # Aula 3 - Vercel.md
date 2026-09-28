# Deploy na Vercel

> **Data de referência:** setembro de 2026  
> Plataformas de hospedagem podem alterar interface, planos, limites e recursos com o tempo. Caso alguma opção esteja em uma posição diferente, consulte a documentação oficial da Vercel.

## Objetivos

Ao final deste material, você deverá ser capaz de:

- entender o que é a Vercel;
- identificar quais tipos de projeto podem ser publicados nela;
- publicar um projeto utilizando GitHub;
- publicar um projeto HTML, CSS e JavaScript;
- publicar um projeto React criado com Vite;
- entender o processo de build;
- atualizar automaticamente uma aplicação utilizando `git push`;
- compreender a diferença entre Production e Preview;
- configurar variáveis de ambiente;
- entender como utilizar um domínio próprio;
- identificar erros comuns de deploy.

# 1. O que é a Vercel?

A **Vercel** é uma plataforma utilizada para publicar aplicações web.

Ela é especialmente conhecida por hospedar aplicações frontend e projetos desenvolvidos com frameworks modernos.

Algumas tecnologias frequentemente utilizadas com a Vercel são:

```text
HTML
CSS
JavaScript
React
Vite
Next.js
Vue
Svelte
```

A Vercel também possui suporte a recursos executados no servidor, principalmente por meio de funções e frameworks compatíveis.

Neste material, o foco será principalmente em:

```text
HTML + CSS + JavaScript

e

React + Vite
```

# 2. O que acontece quando fazemos deploy?

Durante o desenvolvimento, normalmente temos:

```text
MEU COMPUTADOR

localhost:5173
```

Depois do deploy:

```text
MEU COMPUTADOR

GitHub

Vercel

Internet
```

A aplicação passa a possuir uma URL pública semelhante a:

```text
https://nome-do-projeto.vercel.app
```

Qualquer pessoa com acesso à URL poderá abrir o projeto, desde que o deploy esteja configurado como público.

# 3. Vercel e GitHub

Uma das formas mais práticas de utilizar a Vercel é conectá-la ao GitHub.

Nesse caso, o fluxo fica assim:

```text
Projeto local

Git

GitHub

Vercel

Internet
```

Depois que o projeto estiver conectado, novos `push` enviados ao GitHub podem gerar automaticamente novos deploys.

A integração da Vercel suporta GitHub, GitLab e Bitbucket.

# 4. Antes do deploy

Antes de publicar qualquer aplicação, teste o projeto localmente.

Para um projeto HTML simples, abra o `index.html` ou utilize uma extensão como Live Server.

Para um projeto Vite:

```bash
npm run dev
```

Normalmente ele será executado em um endereço semelhante a:

```text
http://localhost:5173
```

Verifique:

```text
a página abre
o CSS funciona
o JavaScript funciona
as imagens carregam
não existem erros importantes no console
```

O deploy não corrige erros existentes no código.

# 5. Primeiro caso: HTML, CSS e JavaScript

Considere esta estrutura:

```text
meu-site

index.html
style.css
script.js
```

Exemplo de `index.html`:

```html
<!DOCTYPE html>
<html lang="pt-BR">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Meu site</title>

    <link rel="stylesheet" href="./style.css">
</head>

<body>

    <h1>Meu primeiro deploy na Vercel</h1>

    <button id="botao">
        Clique aqui
    </button>

    <script src="./script.js"></script>

</body>

</html>
```

Exemplo de `style.css`:

```css
body {
    font-family: Arial, sans-serif;
    text-align: center;
    padding: 40px;
}
```

Exemplo de `script.js`:

```js
const botao = document.querySelector("#botao")

botao.addEventListener("click", () => {
    alert("JavaScript funcionando")
})
```

# 6. Colocando o projeto no GitHub

Se o projeto ainda não possui Git:

```bash
git init
```

Adicione os arquivos:

```bash
git add .
```

Faça o primeiro commit:

```bash
git commit -m "primeiro deploy"
```

Defina a branch principal:

```bash
git branch -M main
```

Crie um repositório no GitHub.

Depois conecte o projeto:

```bash
git remote add origin URL_DO_REPOSITORIO
```

Exemplo:

```bash
git remote add origin https://github.com/usuario/meu-site.git
```

Envie:

```bash
git push -u origin main
```

Agora o código está disponível no GitHub.

# 7. Criando uma conta na Vercel

Acesse o site da Vercel:

```text
https://vercel.com
```

Crie uma conta ou faça login.

Uma opção conveniente é entrar utilizando sua conta do GitHub.

Isso facilita a integração entre as duas plataformas.

# 8. Importando um projeto

Dentro do painel da Vercel, crie um novo projeto.

Procure uma opção semelhante a:

```text
Add New

Project
```

A Vercel exibirá os repositórios disponíveis na sua conta Git.

Localize o projeto desejado.

Depois selecione:

```text
Import
```

Uma tela semelhante à seguinte é utilizada para importar projetos do GitHub:



A Vercel poderá solicitar autorização para acessar seus repositórios do GitHub.

É possível autorizar:

```text
todos os repositórios
```

ou apenas:

```text
repositórios selecionados
```

Para atividades de aula, autorizar apenas os repositórios necessários é uma boa opção.

# 9. Configuração do projeto

Depois de importar o repositório, a Vercel mostrará uma tela de configuração.

Alguns campos importantes podem aparecer:

```text
Project Name
Framework Preset
Root Directory
Build Command
Output Directory
Environment Variables
```

Nem todos precisam ser alterados.

Na maioria dos projetos simples, a Vercel detecta automaticamente boa parte das configurações.

# 10. Project Name

O campo:

```text
Project Name
```

define o nome do projeto dentro da Vercel.

Ele também pode influenciar a URL inicial.

Exemplo:

```text
meu-projeto
```

pode gerar algo semelhante a:

```text
https://meu-projeto.vercel.app
```

Se o nome já estiver sendo utilizado ou não estiver disponível, a plataforma poderá gerar uma variação.

# 11. Framework Preset

A Vercel tenta identificar automaticamente a tecnologia utilizada.

Exemplos:

```text
Vite
Next.js
Vue
Svelte
Other
```

Para um projeto React criado com Vite, normalmente a Vercel detectará:

```text
Vite
```

Se a identificação estiver incorreta, é possível alterar manualmente o **Framework Preset**.

# 12. Root Directory

Normalmente o projeto está diretamente na raiz do repositório:

```text
repositorio

package.json
src
public
```

Nesse caso, não é necessário alterar o `Root Directory`.

Mas alguns repositórios possuem vários projetos:

```text
meu-repositorio

frontend
backend
documentacao
```

Se o frontend estiver dentro de:

```text
frontend
```

o `Root Directory` deverá apontar para essa pasta.

Isso informa à Vercel onde está o projeto que deverá ser construído.

# 13. Fazendo o primeiro deploy

Depois de conferir as configurações, selecione:

```text
Deploy
```

A Vercel iniciará o processo.

Algo semelhante a isto acontecerá:

```text
clonar repositório

instalar dependências

executar build

gerar os arquivos finais

publicar

criar URL
```

Ao final, a plataforma informará se o deploy foi concluído com sucesso.

# 14. A URL do projeto

Depois do deploy, será fornecida uma URL semelhante a:

```text
https://meu-projeto.vercel.app
```

Abra essa URL.

Teste:

```text
página inicial
links
botões
imagens
JavaScript
navegação
responsividade
```

O projeto agora está disponível na internet.

# 15. React com Vite

Agora considere um projeto React criado com:

```bash
npm create vite@latest
```

Uma estrutura típica será:

```text
meu-projeto

node_modules
public
src
.gitignore
index.html
package.json
vite.config.js
```

O arquivo `package.json` normalmente possui scripts parecidos com:

```json
{
    "scripts": {
        "dev": "vite",
        "build": "vite build",
        "preview": "vite preview"
    }
}
```

O script mais importante para o deploy é:

```text
build
```

# 16. O que significa build?

Durante o desenvolvimento utilizamos:

```bash
npm run dev
```

Esse comando cria um servidor de desenvolvimento.

Ele não é a versão final utilizada para produção.

Para gerar a versão de produção:

```bash
npm run build
```

No Vite, normalmente será criada uma pasta:

```text
dist
```

Exemplo:

```text
projeto

src
public
dist
package.json
```

A pasta `dist` contém a versão preparada para publicação.

# 17. Build na Vercel

Quando a Vercel identifica um projeto Vite, normalmente configura automaticamente:

```text
Build Command

npm run build
```

e:

```text
Output Directory

dist
```

Na maioria dos casos não é necessário preencher isso manualmente.

A Vercel detecta frameworks compatíveis e executa o processo de build apropriado.

# 18. Dependências

O arquivo `package.json` informa quais bibliotecas o projeto precisa.

Exemplo:

```json
{
    "dependencies": {
        "react": "...",
        "react-dom": "..."
    }
}
```

Durante o deploy, a Vercel instala as dependências necessárias.

Por isso, normalmente não enviamos:

```text
node_modules
```

para o GitHub.

O `.gitignore` deve conter:

```text
node_modules
```

Em projetos Vite, isso normalmente já é configurado automaticamente.

# 19. Atualizando o projeto

Depois que o projeto está conectado ao GitHub, o processo fica muito mais simples.

Altere algum arquivo.

Depois:

```bash
git add .
```

Faça um commit:

```bash
git commit -m "atualizando página inicial"
```

Envie:

```bash
git push
```

A Vercel detectará o novo código.

O processo passa a ser:

```text
alteração local

commit

push

GitHub

Vercel

novo build

novo deploy
```

Não é necessário criar outro projeto na Vercel.

# 20. Deploy automático

Essa integração representa uma forma de **Continuous Deployment**.

Depois da configuração inicial, cada alteração enviada para a branch configurada pode gerar uma nova versão automaticamente.

Em um fluxo simples:

```text
main

novo push

novo deploy de produção
```

Isso reduz bastante o trabalho manual.

# 21. Production

O ambiente principal é chamado de:

```text
Production
```

É a versão destinada aos usuários finais.

Normalmente a branch principal:

```text
main
```

é utilizada para produção.

Exemplo:

```text
main

Production

https://meu-projeto.vercel.app
```

# 22. Preview

A Vercel também trabalha com deploys de **Preview**.

Imagine as branches:

```text
main
feature-login
feature-dashboard
```

A branch:

```text
main
```

pode representar produção.

Já:

```text
feature-login
```

pode gerar um deploy separado para testes.

A Vercel cria URLs de preview para branches e Pull Requests conectados ao projeto.

Isso permite testar alterações antes de colocá-las em produção.

# 23. Exemplo de fluxo com Preview

Imagine:

```text
main
```

contendo a versão estável.

Criamos:

```bash
git checkout -b feature-login
```

Fazemos alterações.

Depois:

```bash
git add .
git commit -m "criando login"
git push -u origin feature-login
```

A Vercel poderá criar um Preview Deployment.

Assim temos:

```text
Produção

main
```

e:

```text
Preview

feature-login
```

separadamente.

# 24. Por que Preview é útil?

Imagine que uma equipe está alterando o sistema utilizado por uma empresa.

Não seria ideal alterar diretamente a aplicação utilizada pelos clientes.

O Preview permite testar a nova versão antes.

Exemplo:

```text
Produção

versão utilizada pelos usuários
```

```text
Preview

versão nova em desenvolvimento
```

Depois dos testes, as alterações podem ser integradas à `main`.

# 25. Logs de deploy

Quando um deploy falha, a Vercel mostra os logs do processo.

Eles podem informar problemas como:

```text
dependência não encontrada
erro de TypeScript
erro durante npm run build
variável de ambiente ausente
arquivo inexistente
comando incorreto
```

Não basta observar:

```text
Deployment Failed
```

É necessário abrir os logs e encontrar o primeiro erro relevante.

# 26. Testando o build antes do deploy

Uma prática importante é executar localmente:

```bash
npm run build
```

antes de fazer o `push`.

Se aparecer:

```text
Build failed
```

localmente, existe uma grande chance de também falhar na Vercel.

Por isso:

```text
npm run dev
```

não é suficiente para testar uma aplicação antes do deploy.

Também teste:

```bash
npm run build
```

# 27. Um problema comum em TypeScript

Durante o desenvolvimento, o Vite pode continuar funcionando mesmo quando existem determinadas advertências ou situações que serão detectadas durante o build.

Por isso um projeto pode funcionar com:

```bash
npm run dev
```

e falhar com:

```bash
npm run build
```

Sempre confira o build antes da entrega.

# 28. Variáveis de ambiente

Muitos projetos possuem informações que mudam dependendo do ambiente.

Exemplo:

```text
URL da API
chaves de serviços
configurações externas
```

Essas informações podem ser armazenadas utilizando variáveis de ambiente.

Na Vercel, abra o projeto e procure:

```text
Settings

Environment Variables
```

A Vercel permite definir variáveis para diferentes ambientes, como Production, Preview e Development. Depois de alterar variáveis utilizadas pelo projeto, normalmente é necessário realizar um novo deploy para que elas entrem na nova versão.

A tela é semelhante a esta:



# 29. Variável de ambiente no Vite

Em um projeto Vite, variáveis expostas ao código frontend normalmente precisam começar com:

```text
VITE_
```

Exemplo:

```text
VITE_API_URL
```

No código:

```js
const apiUrl = import.meta.env.VITE_API_URL
```

Exemplo:

```js
fetch(`${import.meta.env.VITE_API_URL}/users`)
```

Na Vercel:

```text
Name

VITE_API_URL
```

```text
Value

https://minha-api.com
```

# 30. Arquivo .env

Localmente, podemos utilizar:

```text
.env
```

Exemplo:

```env
VITE_API_URL=http://localhost:3000
```

O `.env` normalmente não deve ser enviado para o GitHub quando contém informações privadas.

Adicione ao `.gitignore`:

```text
.env
```

# 31. Atenção com segredos no frontend

Existe uma diferença importante.

Uma variável utilizada no frontend não se torna secreta apenas porque está em `.env`.

Se o navegador precisa acessar o valor, esse valor pode ser inspecionado pelo usuário.

Portanto, não coloque no frontend:

```text
senha do banco
segredo JWT
senha administrativa
chave privada
credencial de servidor
```

Esses dados devem permanecer em um ambiente seguro do backend.

# 32. Frontend e backend separados

Um projeto pode possuir:

```text
Frontend React

Vercel
```

e:

```text
Backend Node.js

outro servidor
```

Exemplo:

```text
Frontend

https://meu-sistema.vercel.app
```

fazendo requisições para:

```text
Backend

https://api.meu-sistema.com
```

No React:

```js
fetch("https://api.meu-sistema.com/users")
```

É comum utilizar uma variável de ambiente:

```env
VITE_API_URL=https://api.meu-sistema.com
```

e:

```js
fetch(`${import.meta.env.VITE_API_URL}/users`)
```

# 33. localhost não funciona depois do deploy

Um erro muito comum é deixar:

```js
fetch("http://localhost:3000/users")
```

no frontend.

Quando o site está na Vercel, `localhost` passa a se referir ao computador da pessoa que está acessando o site.

A aplicação publicada não encontrará o backend do desenvolvedor dessa forma.

Em produção, utilize o endereço público da API.

Exemplo:

```text
https://api.meu-projeto.com
```

# 34. CORS

Mesmo que frontend e backend estejam funcionando separadamente, o navegador pode bloquear determinadas requisições devido às regras de **Cross-Origin Resource Sharing (CORS)**.

Exemplo:

```text
Frontend

https://meu-site.vercel.app
```

```text
Backend

https://minha-api.com
```

O backend precisa permitir requisições vindas do endereço correto.

Em Express, por exemplo:

```js
import cors from "cors"

app.use(cors({
    origin: "https://meu-site.vercel.app"
}))
```

Durante o desenvolvimento:

```text
http://localhost:5173
```

também pode precisar ser permitido.

# 35. React Router e erro 404

Considere uma aplicação React com rotas:

```text
/
/login
/dashboard
/perfil
```

Ao navegar dentro do React, tudo pode funcionar.

Mas ao acessar diretamente:

```text
https://meu-site.vercel.app/dashboard
```

pode ocorrer erro de rota dependendo da configuração do projeto.

Para uma Single Page Application, pode ser necessário configurar um rewrite.

Um arquivo `vercel.json` pode ser utilizado.

Exemplo:

```json
{
    "rewrites": [
        {
            "source": "/(.*)",
            "destination": "/index.html"
        }
    ]
}
```

Assim, as rotas são entregues ao `index.html`, permitindo que o React Router processe a navegação.

Essa configuração deve ser usada quando fizer sentido para a arquitetura do projeto.

# 36. vercel.json

O arquivo:

```text
vercel.json
```

permite definir configurações específicas do deploy.

Exemplo:

```text
meu-projeto

src
public
package.json
vercel.json
```

Ele pode ser utilizado para recursos como:

```text
rewrites
redirects
headers
configurações de funções
```

Projetos simples normalmente não precisam dele.

# 37. Domínio padrão

Ao publicar um projeto, a Vercel fornece um domínio:

```text
vercel.app
```

Exemplo:

```text
https://meu-projeto.vercel.app
```

Esse endereço pode ser utilizado gratuitamente.

# 38. Domínio próprio

Também é possível utilizar algo como:

```text
https://www.minhaempresa.com.br
```

Dentro do projeto, procure:

```text
Settings

Domains
```

Adicione o domínio desejado.

A Vercel informará quais configurações de Domain Name System (DNS) deverão ser realizadas.

Uma tela de domínio pode apresentar informações semelhantes a estas:



# 39. DNS

Domain Name System (DNS) é o sistema responsável por relacionar nomes como:

```text
www.minhaempresa.com.br
```

aos servidores responsáveis pelo site.

Ao conectar um domínio à Vercel, normalmente será necessário configurar registros de DNS.

A própria Vercel informa quais registros devem ser criados.

Não copie configurações de outro projeto esperando que funcionem automaticamente.

Utilize os valores exibidos para o domínio específico.

# 40. HTTPS

Projetos publicados na Vercel utilizam HTTPS.

Exemplo:

```text
https://meu-projeto.vercel.app
```

HTTPS criptografa a comunicação entre navegador e servidor.

Ao utilizar domínio próprio corretamente configurado, a Vercel também consegue gerenciar o certificado necessário.

# 41. Proteção de deploys

Deploys de Preview podem conter funcionalidades ainda não destinadas ao público.

A Vercel possui recursos de **Deployment Protection**.

É possível exigir autenticação da Vercel para acessar determinados deploys.

Atualmente a Vercel Authentication pode proteger inclusive deployments de produção nos planos disponíveis, dependendo da configuração escolhida.

Para um site público, tome cuidado para não proteger também a URL de produção por engano.

# 42. Deploy sem GitHub

A Vercel também possui outras formas de deploy.

Em 2026, uma delas é o **Vercel Drop**.

Ele permite enviar um arquivo, uma pasta ou um arquivo ZIP diretamente pelo navegador, sem precisar utilizar Git ou a Vercel CLI.

O fluxo é aproximadamente:

```text
projeto

Vercel Drop

Deploy

URL pública
```

Para atividades simples, pode ser útil.

Porém, para projetos em desenvolvimento contínuo, utilizar GitHub costuma ser mais interessante porque permite novos deploys automaticamente após alterações.

# 43. Deploy utilizando Vercel CLI

Também existe a **Vercel CLI**.

Ela pode ser instalada com:

```bash
npm install -g vercel
```

Depois, dentro do projeto:

```bash
vercel
```

Para enviar diretamente para produção:

```bash
vercel --prod
```

A própria documentação da Vercel apresenta esse fluxo como alternativa ao deploy pelo painel.

Para uma primeira experiência, utilizar GitHub e o painel web costuma ser mais simples.

# 44. Comparação dos métodos

| Método | Git necessário | Atualização automática | Uso |
|---|---:|---:|---|
| GitHub + Vercel | Sim | Sim | Projetos em desenvolvimento |
| Vercel Drop | Não | Não pelo Git automaticamente | Deploy rápido |
| Vercel CLI | Não obrigatoriamente | Depende do fluxo | Desenvolvimento e testes |
| GitLab + Vercel | Sim | Sim | Projetos hospedados no GitLab |
| Bitbucket + Vercel | Sim | Sim | Projetos hospedados no Bitbucket |

# 45. Erro: build failed

Se aparecer:

```text
Build Failed
```

abra os logs.

Depois execute localmente:

```bash
npm install
```

e:

```bash
npm run build
```

Corrija os erros apresentados antes de fazer outro deploy.

# 46. Erro: módulo não encontrado

Exemplo:

```text
Module not found
```

Pode indicar:

```text
pacote não instalado
import incorreto
arquivo inexistente
diferença entre maiúsculas e minúsculas
```

Confira o `package.json`.

Se uma biblioteca está sendo utilizada:

```js
import axios from "axios"
```

ela precisa estar instalada:

```bash
npm install axios
```

Depois faça commit do:

```text
package.json
package-lock.json
```

# 47. package-lock.json

Ao utilizar npm, normalmente devemos enviar:

```text
package.json
package-lock.json
```

para o GitHub.

O `package-lock.json` registra versões específicas das dependências utilizadas.

Isso ajuda o servidor de build a instalar um ambiente compatível com o projeto.

# 48. Erro de maiúsculas e minúsculas

No Windows, isto pode funcionar:

```js
import Header from "./components/header"
```

mesmo que o arquivo seja:

```text
Header.jsx
```

Em outros ambientes isso pode falhar.

Prefira:

```js
import Header from "./components/Header"
```

respeitando exatamente o nome do arquivo.

# 49. Erro: imagem não aparece

Evite:

```html
<img src="C:\Users\Aluno\Desktop\foto.png">
```

Esse endereço existe somente no seu computador.

A imagem deve estar dentro do projeto.

Por exemplo:

```text
public

foto.png
```

Em Vite:

```jsx
<img src="/foto.png" />
```

Ou importe o arquivo dentro de `src`.

# 50. Erro: variável de ambiente undefined

Se:

```js
console.log(import.meta.env.VITE_API_URL)
```

retornar:

```text
undefined
```

verifique:

```text
o nome começa com VITE_
a variável foi cadastrada na Vercel
o ambiente correto foi selecionado
foi realizado novo deploy após a alteração
```

A Vercel separa variáveis por ambiente, então uma variável disponível em Production pode não estar disponível em Preview, e vice-versa.

# 51. Erro: API funciona localmente, mas não publicada

Verifique:

```text
URL da API
CORS
HTTPS
variáveis de ambiente
backend realmente publicado
rotas
firewall
```

Se o frontend utiliza:

```text
http://localhost:3000
```

ele provavelmente ainda está apontando para o ambiente local.

# 52. HTTP e HTTPS

Se o frontend está em:

```text
https://
```

e tenta acessar uma API apenas em:

```text
http://
```

o navegador pode bloquear a requisição por segurança.

Em produção, prefira utilizar HTTPS também na API.

# 53. Erro 404 em uma rota React

Situação:

```text
/
```

funciona.

Mas:

```text
/dashboard
```

apresenta:

```text
404
```

Verifique:

```text
React Router
rewrites
vercel.json
```

Em uma Single Page Application tradicional, pode ser necessário encaminhar as rotas para `index.html`.

# 54. Erro após alterar uma variável

Alterar uma variável no painel não significa necessariamente que uma versão já construída da aplicação será reconstruída imediatamente.

Depois de alterar variáveis utilizadas no build, realize um novo deploy.

A documentação atual da Vercel orienta realizar redeploy para que novas variáveis passem a fazer parte do projeto publicado.

# 55. Redeploy

No painel da Vercel, é possível abrir um deployment existente e realizar um novo deploy.

Isso pode ser útil quando:

```text
uma variável foi alterada
uma configuração foi corrigida
o build precisa ser executado novamente
```

Porém, se houve alteração no código, o fluxo recomendado continua sendo:

```bash
git add .
git commit -m "corrigindo projeto"
git push
```

# 56. Histórico de deployments

A Vercel mantém os deployments anteriores do projeto.

Isso ajuda a visualizar:

```text
quando cada versão foi publicada
qual commit gerou o deploy
qual branch foi utilizada
se o build funcionou
qual URL foi gerada
```

Esse histórico é útil para identificar quando um problema começou.

# 57. Fluxo recomendado para um projeto React

Durante o desenvolvimento:

```bash
npm run dev
```

Antes de enviar:

```bash
npm run build
```

Depois:

```bash
git add .
git commit -m "finalizando funcionalidade"
git push
```

A Vercel recebe a alteração e gera o novo deploy.

# 58. Fluxo completo

```text
DESENVOLVIMENTO

VS Code

npm run dev

teste local

npm run build

Git

commit

GitHub

push

Vercel

build

deploy

Internet
```

# 59. Exercício

Crie ou utilize um projeto React com Vite.

O projeto deverá possuir pelo menos:

```text
uma página principal
um componente
estilização
uma imagem
um botão com alguma interação
```

Teste localmente.

Depois execute:

```bash
npm run build
```

Corrija qualquer problema apresentado.

# 60. Publicação

Envie o projeto para o GitHub.

Depois:

1. acesse a Vercel;
2. faça login utilizando GitHub;
3. crie um novo projeto;
4. importe o repositório;
5. confira se o framework foi identificado como Vite;
6. confira as configurações;
7. faça o deploy;
8. abra a URL gerada;
9. teste a aplicação publicada.

# 61. Segunda etapa

Depois do primeiro deploy, altere alguma informação visível.

Por exemplo:

```jsx
<h1>Projeto atualizado</h1>
```

Depois:

```bash
git add .
git commit -m "atualizando página"
git push
```

Acompanhe o novo deploy dentro da Vercel.

Depois abra novamente a URL do projeto e confira a alteração.

# 62. Terceira etapa

Crie uma nova branch:

```bash
git checkout -b feature-teste
```

Faça uma pequena alteração.

Depois:

```bash
git add .
git commit -m "criando alteração de teste"
git push -u origin feature-teste
```

Observe se a Vercel cria um Preview Deployment separado da aplicação principal.

# 63. Requisitos da entrega

Entregue:

```text
link do repositório GitHub

link da aplicação publicada na Vercel
```

Exemplo:

```text
Repositório:

https://github.com/usuario/projeto


Aplicação:

https://projeto.vercel.app
```

# 64. Checklist antes da entrega

Confira:

```text
o projeto funciona localmente
npm run build funciona
o repositório está atualizado
não existe node_modules no GitHub
não existe .env com segredo no GitHub
as imagens carregam
as rotas funcionam
a aplicação abre pela URL da Vercel
o console não apresenta erros importantes
as requisições para API funcionam
```

# 65. Resumo

A Vercel permite transformar:

```text
código no computador
```

em:

```text
aplicação disponível na internet
```

O fluxo mais comum utilizado neste material é:

```text
Projeto local

Git

GitHub

Vercel

Internet
```

Para React com Vite:

```text
npm run dev

desenvolvimento
```

```text
npm run build

versão de produção
```

```text
git push

envio das alterações
```

```text
Vercel

novo deploy
```

Depois da configuração inicial, o processo de atualização se torna simples:

```text
alterar

testar

commit

push

deploy automático
```

Esse fluxo é muito próximo do processo utilizado em projetos reais de desenvolvimento web.
