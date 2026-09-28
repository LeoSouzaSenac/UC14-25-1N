# Introdução a Deploy e Hospedagem

> **Data de referência:** setembro de 2026  
> Alguns serviços de hospedagem alteram planos, limites e preços com frequência. Antes de utilizar um serviço em um projeto comercial, consulte sempre a documentação e a tabela de preços atualizadas.

## Objetivos

Ao final deste material, você deverá ser capaz de:

- explicar o que é deploy;
- diferenciar um projeto rodando localmente de um projeto disponível na internet;
- entender que frontend, backend e banco de dados possuem necessidades diferentes de hospedagem;
- conhecer algumas opções gratuitas e pagas para publicar aplicações;
- entender quando um serviço gratuito é adequado e quando não é;
- compreender o papel do GitHub Pages na publicação de sites estáticos.

## 1. O que significa deploy?

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

### 1.1 Antes do deploy

```text
MEU COMPUTADOR

localhost:5173

Meu site
```

O desenvolvedor consegue acessar.

Outras pessoas na internet, normalmente, não.

### 1.2 Depois do deploy

```text
INTERNET

SERVIDOR

Meu site

https://meusite.com
```

Agora outras pessoas conseguem acessar o sistema.

## 2. Definição de deploy

**Deploy** é o processo de disponibilizar uma versão de uma aplicação em um ambiente onde ela possa ser executada e acessada por seus usuários.

Em português, também podemos encontrar termos como:

- implantação;
- publicação;
- disponibilização.

No mercado de desenvolvimento, entretanto, é extremamente comum utilizar a palavra **deploy**.

Um sistema funcionar corretamente no computador do desenvolvedor não significa que ele já esteja disponível para um cliente ou usuário final.

Ele ainda precisa ser publicado em algum ambiente acessível.

## 3. O que é um servidor?

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

### Exemplo

Quando acessamos:

```text
https://www.exemplo.com
```

nosso navegador faz uma requisição para algum servidor.

O servidor responde com os arquivos ou dados necessários.

```text
NAVEGADOR

requisição HTTP

SERVIDOR

resposta HTTP

NAVEGADOR
```

## 4. Uma aplicação pode possuir várias partes

Nos projetos desenvolvidos durante o curso, normalmente temos algo parecido com:

```text
Frontend
Backend
Banco de dados
```

Cada uma dessas partes possui uma função diferente.

## 5. Frontend

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

### Exemplo

```text
Usuário
Navegador
HTML + CSS + JavaScript
```

## 6. Backend

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

### Exemplo

```text
Frontend
HTTP
Backend
SQL
Banco de dados
```

## 7. Banco de dados

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

## 8. Um sistema completo

Um sistema web pode possuir a seguinte arquitetura:

```text
INTERNET

FRONTEND
HTML ou React

HTTP

BACKEND
Node.js e Express

SQL

BANCO DE DADOS
MySQL
```

## 9. Preciso de três servidores?

Não necessariamente.

É possível colocar frontend, backend e banco em uma única máquina.

Por exemplo, podemos contratar uma **Virtual Private Server (VPS)** e configurar tudo manualmente nela.

```text
VPS

Frontend
Backend Node.js
MySQL
Nginx
```

Também podemos utilizar serviços diferentes para cada parte.

Isso é muito comum atualmente.

```text
Frontend: serviço A
Backend: serviço B
Banco: serviço C
```

## 10. Por que utilizar serviços diferentes?

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

## 11. Hospedagem estática

Uma página feita com:

```text
HTML
CSS
JavaScript
```

pode ser publicada como um **site estático**.

O servidor apenas disponibiliza os arquivos.

```text
index.html
style.css
script.js
imagens/
```

O navegador baixa esses arquivos e executa o JavaScript.

## 12. Onde o JavaScript do frontend executa?

No navegador.

```text
SERVIDOR

index.html
style.css
script.js

NAVEGADOR DO USUÁRIO

executa o JavaScript
```

Isso será importante para entender a diferença entre frontend e backend.

## 13. E o JavaScript do Node.js?

JavaScript utilizado no backend através do Node.js executa **no servidor**.

```text
JavaScript do frontend
Navegador

JavaScript do backend
Servidor Node.js
```

Embora a linguagem possa ser a mesma, o ambiente é diferente.

## 14. GitHub Pages

O **GitHub Pages** é um serviço do GitHub que permite publicar sites estáticos diretamente a partir de um repositório.

Ele funciona bem para projetos como:

- páginas HTML, CSS e JavaScript;
- portfólios;
- currículos;
- páginas de apresentação;
- documentação;
- landing pages;
- exercícios;
- projetos acadêmicos.

## 15. O que o GitHub Pages consegue hospedar?

Exemplo:

```text
index.html
style.css
script.js
```

Esse tipo de projeto funciona no GitHub Pages.

Já aplicações com:

```text
Node.js
Express
TypeORM
MySQL
```

não podem ter esse backend executado diretamente pelo GitHub Pages.

GitHub Pages é voltado principalmente à publicação de conteúdo **estático**.

Ele não é um servidor Node.js.

## 16. GitHub Pages é gratuito?

Para contas GitHub Free, o GitHub Pages pode ser utilizado com repositórios públicos.

Entre suas funcionalidades estão:

- hospedagem de páginas estáticas;
- endereço gratuito utilizando `github.io`;
- HTTPS;
- publicação automática depois de alterações;
- suporte a domínio personalizado;
- publicação por branch;
- integração com GitHub Actions.

O GitHub informa um limite flexível de aproximadamente **100 GB de transferência por mês** para sites do GitHub Pages.

Para os projetos realizados em aula, isso normalmente é suficiente.

## 17. GitHub Pages e projetos comerciais

Existe uma diferença importante entre um projeto de estudo e um projeto comercial.

O próprio GitHub informa que GitHub Pages não deve ser utilizado como hospedagem gratuita para operar lojas virtuais, sistemas de Software as a Service (SaaS) ou sites destinados principalmente a transações comerciais.

Exemplos de uso adequados:

```text
Portfólio
Projeto de aula
Documentação
Landing page simples
Exercícios
```

Exemplos que exigem avaliação de outra infraestrutura:

```text
Sistema comercial complexo
E-commerce
SaaS comercial
```

## 18. Onde hospedar o frontend?

Algumas possibilidades comuns:

| Serviço | Possui opção gratuita? | Uso comum |
|---|---:|---|
| GitHub Pages | Sim | HTML, CSS, JavaScript, documentação e projetos acadêmicos |
| Netlify | Sim | Frontend estático e aplicações frontend |
| Vercel | Sim | React, Next.js e projetos frontend |
| Render Static Sites | Sim | Sites estáticos |
| VPS | Normalmente não | Projetos em que desejamos controlar toda a infraestrutura |

## 19. Onde hospedar o backend?

Algumas opções:

| Serviço | Possui opção gratuita? | Uso comum |
|---|---:|---|
| Render | Sim, com limitações | Node.js, Express e outras APIs |
| Railway | Crédito gratuito limitado | APIs, bancos e serviços |
| VPS | Normalmente paga | Controle completo do servidor |
| Serviços de nuvem | Depende | Projetos profissionais e aplicações maiores |

## 20. Onde hospedar o banco?

Algumas possibilidades:

| Serviço | Banco | Possui opção gratuita? |
|---|---|---:|
| Aiven | MySQL e PostgreSQL | Sim |
| Supabase | PostgreSQL | Sim |
| Neon | PostgreSQL | Sim |
| Railway | Diversos | Crédito limitado |
| Render | PostgreSQL | Sim, com limitações |
| VPS | MySQL, PostgreSQL e outros | A VPS é paga |

## 21. Exemplo gratuito para estudos

Para um projeto utilizado apenas para aprendizagem, poderíamos ter:

```text
Frontend: GitHub Pages

Backend: Render

Banco: Aiven MySQL
```

Essa combinação permite entender claramente que as três partes são serviços separados.

## 22. Aiven MySQL

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

## 23. Render

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
git push
GitHub
Render
novo deploy
```

O Render possui serviços gratuitos voltados a:

- testes;
- aprendizagem;
- hobby;
- protótipos.

A própria documentação do Render recomenda não utilizar os serviços gratuitos para aplicações de produção.

## 24. Railway

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

## 25. Vercel

A Vercel é muito utilizada para projetos frontend, especialmente:

```text
React
Next.js
```

O plano Hobby é gratuito.

Entretanto, atualmente a própria Vercel define esse plano como destinado a uso **pessoal e não comercial**.

Para empresa, freelancer, cliente ou projeto comercial, é necessário avaliar um plano adequado ao uso profissional.

## 26. Netlify

O Netlify também permite publicar aplicações frontend diretamente através de um repositório Git.

Possui:

- deploy automático;
- HTTPS;
- domínio personalizado;
- Content Delivery Network (CDN);
- previews de deploy.

O plano gratuito utiliza atualmente um sistema de créditos mensais.

Quando os créditos terminam, o projeto pode ser pausado até o próximo ciclo.

## 27. Gratuito significa ruim?

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

## 28. Projeto de aula e projeto de cliente

### Projeto de aula

Prioridades:

```text
aprender
testar
entender a arquitetura
não gastar
experimentar
```

Serviços gratuitos normalmente são suficientes.

### Projeto de cliente

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

## 29. Exemplo de arquitetura para um cliente

Uma possibilidade:

```text
www.empresa.com.br

Frontend
Netlify ou Vercel

api.empresa.com.br

Backend
Render, Railway ou outro serviço

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

## 30. Outra possibilidade: VPS

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

Nginx
Frontend
Backend Node.js
MySQL
```

## 31. Qual é a desvantagem da VPS?

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

## 32. HTTPS

Uma URL publicada normalmente utiliza:

```text
https://
```

O **Hypertext Transfer Protocol Secure (HTTPS)** utiliza criptografia para proteger a comunicação entre navegador e servidor.

GitHub Pages fornece HTTPS para seus sites.

## 33. Domínio

Sem comprar um domínio, podemos utilizar um endereço fornecido pela própria plataforma, como:

```text
usuario.github.io
```

Também é possível configurar um domínio próprio.

Por exemplo:

```text
www.minhaempresa.com.br
```

Nesse caso, é necessário possuir o domínio e configurar seu **Domain Name System (DNS)**.

Domínio e DNS podem ser estudados separadamente em outro momento.

## 34. GitHub Pages pode acessar uma API?

Sim.

Um frontend hospedado no GitHub Pages pode realizar requisições para um backend hospedado em outro lugar.

Exemplo:

```text
GitHub Pages
Frontend

fetch()

https://minha-api.onrender.com

Backend Node.js
```

Isso será importante quando o projeto possuir frontend e backend publicados separadamente.

## 35. Arquitetura completa

Um projeto futuro pode possuir uma estrutura semelhante a esta:

```text
USUÁRIO

FRONTEND
React

HTTPS

BACKEND
Express

SQL

BANCO
MySQL
```

Cada parte pode estar disponível na internet em um serviço diferente.

## 36. Resumo

```text
DEPLOY

Disponibilizar a aplicação em um ambiente acessível
```

```text
FRONTEND

HTML
CSS
JavaScript
React
```

```text
BACKEND

Node.js
Express
APIs
```

```text
BANCO

MySQL
PostgreSQL
outros
```

```text
EXEMPLO PARA ESTUDOS

Frontend: GitHub Pages
Backend: Render
Banco: Aiven
```
