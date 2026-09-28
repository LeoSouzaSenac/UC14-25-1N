# GitHub Pages: passo a passo para publicar um site

Este material mostra como publicar uma página feita com HTML, CSS e JavaScript utilizando Git, GitHub e GitHub Pages.

O GitHub Pages é indicado para sites estáticos. Ele não executa um backend Node.js, Express, PHP, Java ou um banco de dados.

## 1. O que será utilizado

```text
HTML
CSS
JavaScript
Git
GitHub
GitHub Pages
```

Neste exemplo não utilizaremos backend.

O processo será:

```text
desenvolver
enviar para o GitHub
publicar com GitHub Pages
acessar pela internet
```

## 2. Criando o projeto

Crie uma pasta chamada:

```text
meu-primeiro-deploy
```

Dentro dela, crie os seguintes arquivos:

```text
meu-primeiro-deploy

index.html
style.css
script.js
```

## 3. Criando o index.html

Crie o arquivo `index.html` com o seguinte conteúdo:

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

        <h1>Meu primeiro deploy</h1>

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

## 4. Criando o style.css

Crie o arquivo `style.css`:

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

## 5. Criando o script.js

Crie o arquivo `script.js`:

```js
const botao = document.querySelector("#botao")

botao.addEventListener("click", () => {
    alert("JavaScript funcionando!")
})
```

## 6. Testando localmente

Antes de publicar, abra a página no navegador e verifique se tudo funciona.

Confira:

```text
HTML carregou?
CSS carregou?
botão funciona?
console possui erros?
```

O deploy não corrige automaticamente um problema que já existe no código local.

## 7. Criando o repositório no GitHub

No GitHub, crie um novo repositório.

Nome sugerido:

```text
meu-primeiro-deploy
```

Para utilizar GitHub Pages gratuitamente com GitHub Free, utilize um repositório público.

## 8. Abrindo o terminal na pasta do projeto

Abra o terminal dentro da pasta `meu-primeiro-deploy`.

Você deve estar na mesma pasta em que estão os arquivos:

```text
index.html
style.css
script.js
```

## 9. Inicializando o Git

Execute:

```bash
git init
```

## 10. Adicionando os arquivos ao Git

Execute:

```bash
git add .
```

## 11. Criando o primeiro commit

Execute:

```bash
git commit -m "primeiro deploy"
```

## 12. Definindo a branch principal

Caso seja necessário, execute:

```bash
git branch -M main
```

Isso garante que a branch principal se chame `main`.

## 13. Conectando o projeto ao repositório do GitHub

Copie a URL do repositório criado no GitHub.

Depois execute:

```bash
git remote add origin URL_DO_REPOSITORIO
```

Exemplo:

```bash
git remote add origin https://github.com/usuario/meu-primeiro-deploy.git
```

## 14. Enviando o projeto para o GitHub

Execute:

```bash
git push -u origin main
```

Depois disso, os arquivos deverão aparecer dentro do repositório no GitHub.

## 15. Ativando o GitHub Pages

No repositório, abra:

```text
Settings
```

Depois abra:

```text
Pages
```

Na seção:

```text
Build and deployment
```

em `Source`, selecione:

```text
Deploy from a branch
```

Na configuração de branch, selecione:

```text
main
```

Na configuração da pasta, selecione:

```text
/(root)
```

Depois clique em:

```text
Save
```

## 16. O que significa /(root)?

Significa que o GitHub deverá procurar os arquivos do site diretamente na raiz do repositório.

Exemplo:

```text
meu-primeiro-deploy

index.html
style.css
script.js
```

O arquivo principal deverá estar ali.

## 17. O arquivo index.html

Para um site HTML comum, é recomendável possuir:

```text
index.html
```

na raiz configurada para publicação.

Para este exemplo, o arquivo de entrada será:

```text
index.html
```

## 18. A URL do site

Um site de projeto normalmente ficará em um endereço semelhante a:

```text
https://usuario.github.io/meu-primeiro-deploy/
```

A publicação pode levar alguns minutos para ficar disponível.

## 19. Conferindo a publicação

Depois que o GitHub Pages terminar o processo, abra a URL do site.

Confira se:

```text
a página abre
o CSS foi carregado
o botão funciona
não existem erros no console
```

## 20. Atualizando o site

Altere alguma informação do `index.html`.

Por exemplo:

```html
<h1>Meu site foi atualizado!</h1>
```

Salve o arquivo.

Depois execute:

```bash
git add .
```

Em seguida:

```bash
git commit -m "atualizando página"
```

Depois:

```bash
git push
```

## 21. O que acontece depois do push?

O fluxo passa a ser:

```text
código alterado
commit
push
GitHub
GitHub Pages
nova versão publicada
```

Esse processo representa uma forma simples de deploy automático.

Você não precisa criar outro site toda vez que alterar seu projeto.

O mesmo site pode ser atualizado várias vezes.

## 22. Erros comuns

### Erro 1: não existe index.html

Estrutura inadequada:

```text
projeto
pagina.html
style.css
```

Estrutura recomendada:

```text
projeto
index.html
style.css
```

### Erro 2: GitHub Pages configurado para outra branch

Exemplo:

```text
Código está na branch: main

Pages está configurado para: gh-pages
```

Nesse caso, o site pode não atualizar corretamente.

Confira:

```text
Settings
Pages
Branch
```

### Erro 3: caminho absoluto

Evite caminhos locais como:

```html
<img src="C:\Users\Aluno\Desktop\foto.png">
```

Esse arquivo existe apenas no seu computador.

Utilize arquivos que estejam dentro do projeto:

```html
<img src="./img/foto.png">
```

### Erro 4: letras maiúsculas e minúsculas

No Windows, isto pode parecer funcionar:

```html
<link rel="stylesheet" href="Style.css">
```

mesmo que o arquivo seja:

```text
style.css
```

Em servidores Linux, diferenças entre letras maiúsculas e minúsculas podem causar problemas.

Prefira manter os nomes exatamente iguais.

### Erro 5: segredos no frontend

Nunca coloque diretamente no JavaScript do frontend informações como:

```text
senha do banco
senha de usuário
chave privada
segredo JWT
credenciais administrativas
```

Tudo enviado ao navegador pode ser inspecionado pelo usuário.

## 23. Exercício

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

## 24. Requisitos da entrega

O repositório deverá conter:

```text
index.html
style.css
script.js
```

A entrega deverá possuir:

```text
link do repositório GitHub
link do site publicado
```

Exemplo:

```text
Repositório:
https://github.com/usuario/projeto

Site:
https://usuario.github.io/projeto/
```

## 25. Segunda etapa do exercício

Depois que o site estiver publicado:

1. altere alguma informação da página;
2. faça um novo commit;
3. envie a alteração utilizando `git push`;
4. aguarde a nova publicação;
5. confira a alteração no site.

O objetivo é observar que o deploy acompanha a evolução do projeto.

## 26. Adicionando uma segunda página

Crie um novo arquivo chamado:

```text
sobre.html
```

No `index.html`, crie um link:

```html
<a href="./sobre.html">
    Sobre
</a>
```

Depois execute:

```bash
git add .
git commit -m "adicionando página sobre"
git push
```

Confira se a nova página também ficou disponível.

## 27. Resumo do processo

```text
criar o projeto
testar localmente
criar o repositório no GitHub
inicializar o Git
criar o primeiro commit
conectar o repositório remoto
fazer o primeiro push
ativar o GitHub Pages
abrir a URL publicada
alterar o projeto
fazer novo commit
fazer novo push
conferir a atualização
```
