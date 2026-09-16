# Aula — Introdução a Deploy e GitHub Pages

> **Data de referência:** setembro de 2026  
> Alguns serviços de hospedagem alteram planos, limites e preços com frequência. Antes de utilizar um serviço em um projeto comercial, consulte sempre a documentação e a tabela de preços atualizadas.

---

# Objetivos da aula

Ao final desta aula, você deverá ser capaz de:

- explicar o que é **deploy**;
- diferenciar um projeto rodando localmente de um projeto disponível na internet;
- entender que **frontend**, **backend** e **banco de dados** possuem necessidades diferentes de hospedagem;
- conhecer algumas opções gratuitas e pagas para publicar aplicações;
- entender quando um serviço gratuito é adequado e quando não é;
- publicar uma página HTML, CSS e JavaScript utilizando **GitHub Pages**;
- atualizar um site já publicado através de `commit` e `push`.

---

# 1. O que significa deploy?

Durante o desenvolvimento de uma aplicação, normalmente executamos o projeto no nosso próprio computador.

Por exemplo:

```text
http://localhost:5173
```

ou:

```text
http://localhost:3000
```

Nesse caso, o projeto está sendo executado **localmente**.

A palavra `localhost` significa, de forma simplificada:

> **este próprio computador**

Por isso, quando uma aplicação funciona em `localhost`, ela não está necessariamente disponível para outras pessoas na internet.

---

## 1.1 Antes do deploy

```text
MEU COMPUTADOR

┌──────────────────────────┐
│                          │
│   localhost:5173         │
│                          │
│   Meu site               │
│                          │
└──────────────────────────┘
```

O desenvolvedor consegue acessar.

Outras pessoas na internet, normalmente, não.

---

## 1.2 Depois do deploy

```text
                 INTERNET

                     │
                     ▼

          ┌────────────────────┐
          │      SERVIDOR      │
          │                    │
          │     Meu site       │
          └────────────────────┘

                     │
                     ▼

          https://meusite.com
```

Agora outras pessoas conseguem acessar o sistema.

---

# 2. Então, o que é deploy?

**Deploy** é o processo de disponibilizar uma versão de uma aplicação em um ambiente onde ela possa ser executada e acessada por seus usuários.

Em português, também podemos encontrar termos como:

- implantação;
- publicação;
- disponibilização.

No mercado de desenvolvimento, entretanto, é extremamente comum utilizar a palavra **deploy**.

---

## Pergunta para a turma

> Se meu sistema funciona perfeitamente no meu computador, significa que ele já está pronto para ser utilizado por um cliente?

Não.

O sistema ainda precisa ser publicado em algum ambiente acessível aos usuários.

---

# 3. O que é um servidor?

Um **servidor** é um computador ou serviço que disponibiliza algum recurso para outros computadores.

Um servidor pode disponibilizar:

- páginas;
- arquivos;
- imagens;
- uma API;
- dados;
- autenticação;
- banco de dados;
- vídeos;
- jogos;
- entre muitos outros recursos.

---

## Exemplo

Quando acessamos:

```text
https://www.exemplo.com
```

nosso navegador faz uma **requisição** para algum servidor.

O servidor responde com os arquivos ou dados necessários.

```text
NAVEGADOR                    SERVIDOR

    │                            │
    │ -------- requisição -----> │
    │                            │
    │ <--------- resposta ------ │
    │                            │
```

---

# 4. Uma aplicação pode possuir várias partes

Nos projetos desenvolvidos durante o curso, normalmente temos algo parecido com:

```text
Frontend
Backend
Banco de dados
```

Cada uma dessas partes possui uma função diferente.

---

# 5. Frontend

O **frontend** é a parte da aplicação com a qual o usuário interage.

Exemplos de tecnologias:

```text
HTML
CSS
JavaScript
React
Vue
Angular
```

Em uma aplicação web, o frontend normalmente será executado no **navegador do usuário**.

---

## Exemplo

```text
Usuário
   │
   ▼
Navegador
   │
   ▼
HTML + CSS + JavaScript
```

---

# 6. Backend

O **backend** executa regras e operações que normalmente não devem ficar diretamente no navegador.

Exemplos:

```text
Node.js
Express
Java
Spring
C#
.NET
PHP
Python
Django
```

Um backend pode ser responsável por:

- autenticação;
- cadastro de usuários;
- regras de negócio;
- validações;
- acesso ao banco;
- permissões;
- geração de relatórios;
- integração com outros sistemas.

---

## Exemplo

```text
Frontend
   │
   │ HTTP
   ▼
Backend
   │
   ▼
Banco de dados
```

---

# 7. Banco de dados

O banco de dados armazena informações utilizadas pela aplicação.

Exemplos:

```text
MySQL
PostgreSQL
MariaDB
SQL Server
MongoDB
```

Ele pode guardar informações como:

```text
usuários
produtos
pedidos
postagens
vagas
mensagens
notas
estoque
```

---

# 8. Um sistema completo

Um sistema web pode possuir a seguinte arquitetura:

```text
                     INTERNET

                         │

                         ▼

                ┌────────────────┐
                │    FRONTEND    │
                │                │
                │ HTML / React   │
                └───────┬────────┘
                        │
                        │ HTTP
                        │
                        ▼
                ┌────────────────┐
                │    BACKEND     │
                │                │
                │ Node/Express   │
                └───────┬────────┘
                        │
                        │ SQL
                        │
                        ▼
                ┌────────────────┐
                │ BANCO DE DADOS │
                │                │
                │     MySQL      │
                └────────────────┘
```

---

# 9. Preciso de três servidores?

Não necessariamente.

É possível colocar:

```text
frontend
backend
banco
```

em uma única máquina.

Por exemplo, podemos contratar uma **Virtual Private Server (VPS)** e configurar tudo manualmente nela.

```text
VPS

┌──────────────────────────────┐
│                              │
│ Frontend                     │
│ Backend Node                 │
│ MySQL                        │
│ Nginx                        │
│                              │
└──────────────────────────────┘
```

Mas também podemos utilizar serviços diferentes para cada parte.

Isso é muito comum atualmente.

```text
Frontend  → serviço A
Backend   → serviço B
Banco     → serviço C
```

---

# 10. Por que utilizar serviços diferentes?

Cada parte possui necessidades diferentes.

Um frontend feito somente com HTML, CSS e JavaScript precisa principalmente que seus **arquivos sejam entregues ao navegador**.

Um backend precisa que um programa permaneça executando.

Por exemplo:

```bash
npm start
```

ou:

```bash
node dist/app.js
```

Um banco de dados, por sua vez, precisa de um sistema próprio para armazenar e consultar dados.

Por isso existem serviços especializados.

---

# 11. Hospedagem estática

Uma página feita com:

```text
HTML
CSS
JavaScript
```

pode ser publicada como um **site estático**.

O servidor apenas disponibiliza os arquivos.

```text
Servidor

├── index.html
├── style.css
├── script.js
└── imagens
```

O navegador baixa esses arquivos e executa o JavaScript.

---

# 12. Onde o JavaScript do frontend executa?

No navegador.

```text
SERVIDOR

index.html
style.css
script.js

      │
      ▼

NAVEGADOR DO USUÁRIO

executa o JavaScript
```

Isso será importante para entender a diferença entre frontend e backend.

---

# 13. E o JavaScript do Node.js?

JavaScript utilizado no backend através do Node.js executa **no servidor**.

```text
JavaScript do frontend
        ↓
Navegador


JavaScript do backend
        ↓
Servidor Node.js
```

Embora a linguagem possa ser a mesma, o ambiente é diferente.

---

# 14. GitHub Pages

O **GitHub Pages** é um serviço do GitHub que permite publicar sites estáticos diretamente a partir de um repositório.

Ele funciona muito bem para projetos como:

- páginas HTML/CSS/JS;
- portfólios;
- currículos;
- páginas de apresentação;
- documentação;
- landing pages;
- exercícios;
- projetos acadêmicos.

---

# 15. O que o GitHub Pages consegue hospedar?

Exemplo:

```text
index.html
style.css
script.js
```

✅ Funciona.

---

## E isso?

```text
Node.js
Express
TypeORM
MySQL
```

❌ O GitHub Pages não executa esse backend.

GitHub Pages é voltado principalmente à publicação de conteúdo **estático**.

Ele não é um servidor Node.js.

---

# 16. GitHub Pages é gratuito?

Para contas GitHub Free, o GitHub Pages pode ser utilizado com repositórios públicos.

Entre suas funcionalidades estão:

- hospedagem de páginas estáticas;
- endereço gratuito utilizando `github.io`;
- HTTPS;
- publicação automática depois de alterações;
- suporte a domínio personalizado;
- publicação por branch;
- integração com GitHub Actions.

O GitHub informa atualmente um limite flexível de aproximadamente **100 GB de transferência por mês** para sites do GitHub Pages.

Para os projetos realizados em aula, isso é muito mais do que normalmente será necessário.

---

# 17. GitHub Pages e projetos comerciais

Existe uma diferença importante entre:

```text
projeto de estudo
```

e:

```text
projeto comercial
```

O próprio GitHub informa que GitHub Pages não deve ser utilizado como hospedagem gratuita para operar lojas virtuais, sistemas de Software as a Service (SaaS) ou sites destinados principalmente a transações comerciais.

Portanto:

```text
Portfólio                    ✅

Projeto de aula              ✅

Documentação                 ✅

Landing page simples         ✅

Exercícios                   ✅

Sistema comercial complexo  ⚠️

E-commerce                   ❌ como hospedagem gratuita principal

SaaS comercial               ❌ como hospedagem gratuita principal
```

---

# 18. Onde hospedar o frontend?

Algumas possibilidades comuns:

| Serviço | Possui opção gratuita? | Uso comum |
|---|---:|---|
| GitHub Pages | Sim | HTML, CSS, JS, documentação e projetos acadêmicos |
| Netlify | Sim | Frontend estático e aplicações frontend |
| Vercel | Sim | React, Next.js e projetos frontend |
| Render Static Sites | Sim | Sites estáticos |
| VPS | Normalmente não | Projetos em que desejamos controlar toda a infraestrutura |

---

# 19. Onde hospedar o backend?

Algumas opções:

| Serviço | Possui opção gratuita? | Uso comum |
|---|---:|---|
| Render | Sim, com limitações | Node.js, Express e outras APIs |
| Railway | Crédito gratuito limitado | APIs, bancos e serviços |
| VPS | Normalmente paga | Controle completo do servidor |
| Serviços de nuvem | Depende | Projetos profissionais e aplicações maiores |

---

# 20. Onde hospedar o banco?

Algumas possibilidades:

| Serviço | Banco | Possui opção gratuita? |
|---|---|---:|
| Aiven | MySQL / PostgreSQL | Sim |
| Supabase | PostgreSQL | Sim |
| Neon | PostgreSQL | Sim |
| Railway | Diversos | Crédito limitado |
| Render | PostgreSQL | Sim, com limitações |
| VPS | MySQL/PostgreSQL/etc. | A VPS é paga |

---

# 21. Exemplo gratuito para estudos

Para um projeto utilizado apenas para aprendizagem, poderíamos ter:

```text
FRONTEND
GitHub Pages
      │
      │ requisição HTTP
      ▼
BACKEND
Render
      │
      │ SQL
      ▼
BANCO
Aiven MySQL
```

Essa combinação permite entender claramente que as três partes são serviços separados.

---

# 22. Aiven MySQL

Em setembro de 2026, a Aiven oferece um plano gratuito para MySQL voltado a:

- aprendizagem;
- protótipos;
- demonstrações;
- testes.

O plano gratuito informado pela empresa atualmente possui aproximadamente:

```text
1 CPU
1 GB RAM
1 GB de armazenamento
```

Ele também possui limitações e não oferece o mesmo nível de garantia e suporte de um plano de produção.

Para uma aula ou projeto simples, entretanto, é uma alternativa interessante.

---

# 23. Render

O Render permite publicar aplicações como:

```text
Node.js
Python
Ruby
Go
Rails
```

Também consegue publicar sites estáticos.

Um projeto pode ser conectado a um repositório GitHub.

Depois disso, alterações enviadas para o repositório podem gerar novos deploys.

```text
Código
  │
  ▼
git push
  │
  ▼
GitHub
  │
  ▼
Render
  │
  ▼
novo deploy
```

O Render possui serviços gratuitos voltados a:

- testes;
- aprendizagem;
- hobby;
- protótipos.

A própria documentação do Render recomenda **não utilizar os serviços gratuitos para aplicações de produção**.

---

# 24. Railway

O Railway permite criar serviços como:

```text
Backend Node.js
Banco PostgreSQL
Banco MySQL
Redis
Workers
```

Em setembro de 2026, novos usuários podem receber um crédito inicial de teste.

Após o período de teste, o plano gratuito possui uma pequena quantidade de crédito mensal.

Portanto, ele pode ser útil em aulas e experimentos, mas exige atenção ao consumo.

---

# 25. Vercel

A Vercel é muito utilizada para projetos frontend, especialmente:

```text
React
Next.js
```

O plano Hobby é gratuito.

Entretanto, atualmente a própria Vercel define esse plano como destinado a uso **pessoal e não comercial**.

Para:

```text
empresa
freelancer
cliente
projeto comercial
```

é necessário avaliar um plano adequado ao uso profissional.

---

# 26. Netlify

O Netlify também permite publicar aplicações frontend diretamente através de um repositório Git.

Possui:

- deploy automático;
- HTTPS;
- domínio personalizado;
- Content Delivery Network (CDN);
- previews de deploy.

O plano gratuito utiliza atualmente um sistema de créditos mensais.

Quando os créditos terminam, o projeto pode ser pausado até o próximo ciclo.

---

# 27. Gratuito significa ruim?

Não.

Serviços gratuitos são excelentes para:

```text
estudo
portfólio
projeto acadêmico
teste
protótipo
demonstração
```

O problema aparece quando utilizamos uma infraestrutura gratuita para algo que exige:

```text
alta disponibilidade
suporte
backup confiável
garantia de desempenho
muitos usuários
monitoramento
recuperação de falhas
acordos de disponibilidade
```

---

# 28. Projeto de aula x projeto de cliente

## Projeto de aula

Prioridades:

```text
aprender
testar
entender a arquitetura
não gastar
experimentar
```

Serviços gratuitos normalmente são suficientes.

---

## Projeto de cliente

As prioridades mudam.

Precisamos pensar em:

```text
segurança
backup
disponibilidade
desempenho
domínio
HTTPS
monitoramento
logs
suporte
recuperação
custos
escalabilidade
```

---

# 29. Exemplo de arquitetura para um cliente

Uma possibilidade:

```text
www.empresa.com.br
        │
        ▼
Frontend
Netlify / Vercel
        │
        ▼
api.empresa.com.br
        │
        ▼
Backend
Render / Railway / outro serviço
        │
        ▼
Banco gerenciado
```

Não existe uma única hospedagem correta para todos os projetos.

A escolha depende de:

```text
quantidade de usuários
tecnologias utilizadas
orçamento
tipo de sistema
necessidade de disponibilidade
volume de dados
segurança
equipe responsável
```

---

# 30. Outra possibilidade: VPS

Uma **Virtual Private Server (VPS)** fornece uma máquina virtual que fica disponível na internet.

Nela podemos instalar manualmente:

```text
Linux
Node.js
MySQL
Nginx
Docker
```

E colocar várias partes da aplicação no mesmo servidor.

```text
                   VPS

        ┌────────────────────────┐
        │                        │
        │ Nginx                  │
        │                        │
        │ Frontend               │
        │                        │
        │ Backend Node.js        │
        │                        │
        │ MySQL                  │
        │                        │
        └────────────────────────┘
```

---

# 31. Qual é a desvantagem da VPS?

Maior controle significa também maior responsabilidade.

Se administrarmos uma VPS, poderemos precisar cuidar de:

- sistema operacional;
- atualizações;
- firewall;
- portas;
- HTTPS;
- domínio;
- proxy reverso;
- banco;
- backups;
- logs;
- monitoramento;
- segurança;
- reinicialização dos serviços.

Serviços como Render, Railway, Vercel e Netlify automatizam parte desse trabalho.

---

# 32. Primeiro deploy da turma

Nesta aula utilizaremos:

```text
HTML
CSS
JavaScript
Git
GitHub
GitHub Pages
```

Ainda não utilizaremos backend.

Nosso objetivo inicial é entender o processo:

```text
desenvolver
    ↓
enviar para o GitHub
    ↓
publicar
    ↓
acessar pela internet
```

---

# 33. Projeto

Crie uma pasta chamada:

```text
meu-primeiro-deploy
```

Dentro dela:

```text
meu-primeiro-deploy
│
├── index.html
├── style.css
└── script.js
```

---

# 34. index.html

```html
<!DOCTYPE html>
<html lang="pt-BR">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Meu primeiro deploy</title>

    <link rel="stylesheet" href="style.css">
</head>

<body>

    <main>

        <h1>Meu primeiro deploy 🚀</h1>

        <p>
            Esta página está disponível na internet.
        </p>

        <button id="botao">
            Clique aqui
        </button>

    </main>

    <script src="script.js"></script>

</body>

</html>
```

---

# 35. style.css

```css
body {
    margin: 0;

    min-height: 100vh;

    display: flex;
    justify-content: center;
    align-items: center;

    font-family: Arial, sans-serif;

    background: #111827;
    color: white;
}

main {
    text-align: center;
}

button {
    padding: 12px 20px;

    border: none;
    border-radius: 8px;

    font-size: 16px;

    cursor: pointer;
}
```

---

# 36. script.js

```js
const botao = document.querySelector("#botao")

botao.addEventListener("click", () => {

    alert("JavaScript funcionando!")

})
```

---

# 37. Teste local

Antes de fazer deploy, abra a página no navegador e verifique se ela funciona.

Confira:

```text
HTML carregou?
CSS carregou?
botão funciona?
console possui erros?
```

Nunca faça deploy esperando que o servidor corrija um código que já está com problema localmente.

---

# 38. Criando o repositório

No GitHub, crie um novo repositório.

Nome sugerido:

```text
meu-primeiro-deploy
```

Para utilizar GitHub Pages gratuitamente com GitHub Free, utilize um repositório público.

---

# 39. Inicializando o Git

Abra o terminal na pasta do projeto.

```bash
git init
```

Adicione os arquivos:

```bash
git add .
```

Crie o primeiro commit:

```bash
git commit -m "primeiro deploy"
```

---

# 40. Definindo a branch principal

Caso necessário:

```bash
git branch -M main
```

---

# 41. Conectando ao GitHub

Copie a URL do repositório criado no GitHub.

Depois:

```bash
git remote add origin URL_DO_REPOSITORIO
```

Exemplo de estrutura:

```bash
git remote add origin https://github.com/usuario/meu-primeiro-deploy.git
```

---

# 42. Enviando o projeto

```bash
git push -u origin main
```

Agora os arquivos estarão no GitHub.

---

# 43. Ativando GitHub Pages

No repositório, abra:

```text
Settings
```

Depois:

```text
Pages
```

Em:

```text
Build and deployment
```

selecione:

```text
Source:
Deploy from a branch
```

Escolha:

```text
Branch:
main
```

E a pasta:

```text
/(root)
```

Depois clique em:

```text
Save
```

---

# 44. O que significa /(root)?

Significa que o GitHub deverá procurar os arquivos do site diretamente na raiz do repositório.

Exemplo:

```text
meu-primeiro-deploy
│
├── index.html
├── style.css
└── script.js
```

O arquivo principal deverá estar ali.

---

# 45. index.html

Para um site HTML comum, é recomendável possuir:

```text
index.html
```

na raiz configurada para publicação.

O GitHub Pages procura um arquivo de entrada como:

```text
index.html
index.md
README.md
```

Para nossa atividade utilizaremos:

```text
index.html
```

---

# 46. URL do site

Um site de projeto normalmente ficará em um endereço semelhante a:

```text
https://usuario.github.io/meu-primeiro-deploy/
```

O GitHub pode levar alguns minutos para finalizar a publicação.

---

# 47. Agora o site está na internet

Antes:

```text
Meu computador

index.html
```

Depois:

```text
Meu computador
      │
      │ git push
      ▼
GitHub
      │
      ▼
GitHub Pages
      │
      ▼
Internet
```

---

# 48. Modificando o site

Altere o texto do HTML.

Por exemplo:

```html
<h1>Meu site foi atualizado!</h1>
```

Salve.

Depois:

```bash
git add .
```

```bash
git commit -m "atualizando página"
```

```bash
git push
```

---

# 49. O que acontece depois do push?

```text
Código alterado
      │
      ▼
commit
      │
      ▼
push
      │
      ▼
GitHub
      │
      ▼
GitHub Pages
      │
      ▼
nova versão publicada
```

Esse processo já representa uma forma simples de **deploy automático**.

---

# 50. Uma ideia importante

Você não precisa criar outro site toda vez que alterar seu projeto.

Você atualiza o mesmo projeto.

```text
versão 1
   │
   ▼
alteração
   │
   ▼
versão 2
   │
   ▼
alteração
   │
   ▼
versão 3
```

O Git ajuda a controlar as versões.

A plataforma de hospedagem publica a versão desejada.

---

# 51. HTTPS

Observe a URL do GitHub Pages:

```text
https://
```

O **Hypertext Transfer Protocol Secure (HTTPS)** utiliza criptografia para proteger a comunicação entre navegador e servidor.

GitHub Pages fornece HTTPS para seus sites.

---

# 52. Domínio

Sem comprar um domínio, podemos utilizar:

```text
usuario.github.io
```

Também é possível configurar um domínio próprio.

Por exemplo:

```text
www.minhaempresa.com.br
```

Nesse caso, é necessário possuir o domínio e configurar seu **Domain Name System (DNS)**.

Veremos domínio e DNS em outro momento.

---

# 53. Erros comuns

## Erro 1 — Não existe index.html

Estrutura errada:

```text
projeto
├── pagina.html
└── style.css
```

Estrutura recomendada:

```text
projeto
├── index.html
└── style.css
```

---

## Erro 2 — GitHub Pages está configurado para outra branch

Exemplo:

```text
Código está na branch: main

Pages está procurando: gh-pages
```

Resultado:

```text
site não atualiza corretamente
```

Confira:

```text
Settings
→ Pages
→ Branch
```

---

## Erro 3 — Caminho absoluto

Evite caminhos locais como:

```html
<img src="C:\Users\Aluno\Desktop\foto.png">
```

Esse arquivo existe apenas no seu computador.

Utilize arquivos que estejam dentro do projeto:

```html
<img src="./img/foto.png">
```

---

## Erro 4 — Letras maiúsculas e minúsculas

No Windows, isto pode parecer funcionar:

```html
<link rel="stylesheet" href="Style.css">
```

mesmo que o arquivo seja:

```text
style.css
```

Em servidores Linux, diferenças entre maiúsculas e minúsculas podem causar problemas.

Prefira manter os nomes exatamente iguais.

---

## Erro 5 — Segredos no frontend

Nunca coloque diretamente no JavaScript do frontend informações como:

```text
senha do banco
senha de usuário
chave privada
segredo JWT
credenciais administrativas
```

Tudo enviado ao navegador pode ser inspecionado pelo usuário.

---

# 54. GitHub Pages pode acessar uma API?

Sim.

Um frontend hospedado no GitHub Pages pode realizar requisições para um backend hospedado em outro lugar.

Exemplo:

```text
GitHub Pages
Frontend
     │
     │ fetch()
     ▼
https://minha-api.onrender.com
     │
     ▼
Backend Node.js
```

Isso será trabalhado quando fizermos o deploy do backend.

---

# 55. Arquitetura que veremos futuramente

```text
                 USUÁRIO

                    │

                    ▼

             ┌────────────┐
             │  FRONTEND  │
             │            │
             │   React    │
             └─────┬──────┘
                   │
                   │ HTTPS
                   ▼
             ┌────────────┐
             │  BACKEND   │
             │            │
             │  Express   │
             └─────┬──────┘
                   │
                   │ SQL
                   ▼
             ┌────────────┐
             │   BANCO    │
             │            │
             │   MySQL    │
             └────────────┘
```

Cada parte estará disponível na internet.

---

# 56. Exercício

Crie uma página pessoal simples contendo:

```text
nome
curso
uma pequena apresentação
três tecnologias que você conhece
um botão
uma imagem
um link para seu GitHub
```

Utilize:

```text
HTML
CSS
JavaScript
```

Depois publique o projeto utilizando **GitHub Pages**.

---

# 57. Requisitos da entrega

O repositório deverá conter:

```text
index.html
style.css
script.js
```

O aluno deverá entregar:

```text
link do repositório GitHub

e

link do site publicado
```

Exemplo:

```text
Repositório:
https://github.com/usuario/projeto

Site:
https://usuario.github.io/projeto/
```

---

# 58. Segunda etapa do exercício

Depois que o site estiver publicado:

1. altere alguma informação da página;
2. faça um novo commit;
3. envie utilizando `git push`;
4. aguarde a nova publicação;
5. confira a alteração no site.

Objetivo:

> perceber que o deploy acompanha a evolução do projeto.

---

# 59. Desafio

Adicione uma nova página:

```text
sobre.html
```

Crie um link no `index.html`:

```html
<a href="./sobre.html">
    Sobre
</a>
```

Depois faça novamente:

```bash
git add .
git commit -m "adicionando página sobre"
git push
```

Confira se a nova página também ficou disponível.

---

# 60. Perguntas de revisão

### 1. O que é deploy?

Processo de disponibilizar uma aplicação em um ambiente onde seus usuários possam acessá-la.

---

### 2. O que significa localhost?

É uma referência ao próprio computador em que o programa está sendo executado.

---

### 3. GitHub Pages executa Node.js?

Não.

GitHub Pages é voltado principalmente para sites estáticos.

---

### 4. Onde o JavaScript de uma página HTML normalmente executa?

No navegador do usuário.

---

### 5. Onde o Node.js executa?

No servidor.

---

### 6. Frontend, backend e banco precisam obrigatoriamente estar na mesma máquina?

Não.

Eles podem estar juntos ou separados.

---

### 7. Por que não devemos escolher uma hospedagem somente porque ela é gratuita?

Porque projetos reais também precisam considerar disponibilidade, segurança, suporte, backup, desempenho, limites de uso e custos futuros.

---

### 8. O que acontece quando fazemos um novo push para a branch utilizada pelo GitHub Pages?

Uma nova versão do site pode ser publicada automaticamente.

---

# 61. Resumo

```text
DEPLOY
│
└── colocar a aplicação em um ambiente acessível
```

```text
FRONTEND
│
├── HTML
├── CSS
├── JavaScript
└── React
```

```text
BACKEND
│
├── Node.js
├── Express
└── APIs
```

```text
BANCO
│
├── MySQL
├── PostgreSQL
└── outros
```

```text
EXEMPLO PARA ESTUDOS

Frontend → GitHub Pages
Backend  → Render
Banco    → Aiven
```

E nesta aula:

```text
HTML/CSS/JS
      │
      ▼
Git
      │
      ▼
GitHub
      │
      ▼
GitHub Pages
      │
      ▼
Internet
```

---



