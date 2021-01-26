---
layout: disciplina
title: GIT-COURSE
subtitle: Minicurso git e git hub 2021
---

# COMANDOS
```console
pwd
# mostra o caminho para o seu diretorio atual

ls
# lista arquivos e diretorios
# ls -a lista arquivos ocultos

mkdir
# cria diretório

cd
# acessa diretório
# cd .. volta para o diretorio anterior

rm
# remove arquivo ou diretório (use -R para remover de forma recursiva)

git init
# inicializa um repositorio

git status
# mostra a situação do seu repositorio

git log
# Exibe a uma lista de commits executados
# git log --oneline
# git log --stat
# git log -p

git show
# Exibe os dados de um commit

git clone
# copia um repositório para o seu computador

$git add 
# adiciona arquivos do diretório de trabalho na staging area

$git commit 
# Envia os arquivos da staging area para o repositório

$git diff 
# Exibe as diferenças entre duas versões do arquivo

```


# EXERCICIOS:

## Inicializando repositorio
```console
# cria diretório git-course
mkdir git-course

#acessa diretório git-course
cd git-course 

# cria diretório new-git-project
mkdir new-git-project

# acessa diretório new-git-project
cd new-git-project

# inicializa repositorio
git init

# Resultado: 
# Initialized empty Git repository in /.../new-git-project/.git

# Apresenta a situação do diretório de trabalho em relação ao repo
git status

# Resultado: 
# On branch main
# No commits yet
# nothing to commit (create/copy files and use "git add" to track)
```

## Primeiro Commit o index.html, app.css e app.js:

Crie um arquivo index.html
Crie um diretório css e dentro o arquivo app.css
Crie um diretório js e dentro um arquivo app.js

Use o editor de sua preferencia

```console
|- index.html
|- css \ app.css
|- js \ app.js
```
Conteudo do arquivo HTML:
```html
<!doctype html>
<html lang="pt-BR">
<head>
    <meta charset="utf-8">
    <title>Projeto Blog</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="">
    <link rel="stylesheet" href="css/app.css">
</head>
<body>

    <script src="js/app.js"></script>
</body>
</html>
```

```console
# Apresenta a situação do diretório de trabalho em relação ao repo
git status

# adiciona o arquivo index.html no stage index para preparar o commit
git add index.html

# adiciona os arquivos app.ccs app.js no stage index para preparar o commit
git add app.ccs app.js

# Apresenta a situação do diretório de trabalho em relação ao repo
git status

# envia o commit da stage index ao repositorio (local) 
git commit -m "adiciona arquivos iniciais"

# Apresenta a situação do diretório de trabalho em relação ao repo
git status
```

## Segundo Commit altera o index.html:

Altere o conteudo no arquivo index.html dentro da tag <body> ... </body>:
```html
    <header>
        <h1> O nome do seu blog </h1>
    </header>
```

```console
# Apresenta a situação do diretório de trabalho em relação ao repo
git status

# adiciona todos os arquivos modificados no stage index para preparar o commit
git add .

# envia o commit da stage index ao repositorio (local) 
git commit -m "altera o index.html adiciona o header"

# Apresenta a situação do diretório de trabalho em relação ao repo
git status
```

## Terceiro Commit gitignore:

Crie um arquivo .gitignore com o conteudo. Lista de arquivos que devem ser ignorados pelo git.
##### ATENÇÃO o ponto(.) na frente do nome do arquivo é obrigatorio e torna ele oculto.

Conteudo do .gitignore:
```
*.class

```

```console
# Apresenta a situação do diretório de trabalho em relação ao repo
git status

# adiciona todos os arquivos modificados no stage index para preparar o commit
git add .

# envia o commit da stage index ao repositorio (local) 
git commit -m "adiciona gitignore"

# Apresenta a situação do diretório de trabalho em relação ao repo
git status
```


## Tag Adcionando uma tag v1.0

```console

#adiciona um label (tag) ao ultimo commit
git tag v1.0

#mostra a lista de commits em uma linha cada
git log -- oneline
```

Adicione ao index.html dentro do <body>

```html
  <div class="container">
        <main>
        </main>
    </div>

    <footer>
        Feito com ♥ 
    </footer>

```

```console
# Apresenta a situação do diretório de trabalho em relação ao repo
git status

# adiciona todos os arquivos modificados no stage index para preparar o commit
git add .

# envia o commit da stage index ao repositorio (local) 
git commit -m "altera o index.html adiciona o main"

# Apresenta a situação do diretório de trabalho em relação ao repo
git status

#mostra a lista de commits em uma linha cada 
git log -- oneline
#verifique que a tag continua no commit anterior

```

## Branchs

crie uma branch sidebar
```console
# cria a branch sidebar
git branch sidebar

# mostra a lista de commits em uma linha cada 
git log --oneline --graph --all
# verifique que a tag e as branchs
```

Ainda na brach main adicione ao app.css:

```css
  body {
    background-color: #00cae4;
  }
```

```console
# Apresenta a situação do diretório de trabalho em relação ao repo
git status

# adiciona todos os arquivos modificados no stage index para preparar o commit
git add .

# envia o commit da stage index ao repositorio (local) 
git commit -m "css atualiza cor"

# mostra a lista de commits em uma linha cada 
git log --oneline --graph --all
# verifique que a tag e as branchs

```

Mude de branch e altere o index.html

```console
# troca para branch sidebar
git checkout sidebar
```

Na brach sidebar adicione ao index.html dentro da   <div class="container"> e depois da tag </main>:

```html
<aside>
    <h2>About Me</h2>

    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Eos, debitis earum molestias veniam suscipit aliquam totam exercitationem tempore neque vitae. Minima, corporis pariatur facere at quo porro beatae similique! Odit.</p>
</aside>

```

```console
# Apresenta a situação do diretório de trabalho em relação ao repo
git status

# adiciona todos os arquivos modificados no stage index para preparar o commit
git add .

# envia o commit da stage index ao repositorio (local) 
git commit -m "atualiza sidebar"

# mostra a lista de commits em uma linha cada 
git log --oneline --graph --all
# verifique que a tag e as branchs
```

Crie a branch footer apartir da main e altere index.html na tag <footer>:

```console
# Cria a branch footer iniciando da main e alterna para branch footer
# equivalente a:
# git checkout main - precisaria desse para sair da branch sidebar e ir para main
# git branch footer - na main cria a branch footer
# git checkout footer - alternar para branch footer
# -b cria a banch e altera para ela
# footer <é a nova branch> e main <é a origem>
$ git checkout -b footer main

# mostra a lista de commits em uma linha cada 
git log --oneline --graph --all
# verifique que a tag e as branchs
```

Altere o arquivo index.html na tag footer
```html
<footer>
    <section>
        <h3 class="visuallyhidden">Social Links</h3>
        <a class="social-link" href="https://twitter.com/mariojp">
            <img src="img/social-twitter.png" alt="Twitter">
        </a>
        <a class="social-link" href="https://www.instagram.com/mariojpereira/">
            <img src="img/social-instagram.png" alt="Instagram">
        </a>
    </section>
</footer>

```

```console

# adiciona todos os arquivos modificados no stage index para preparar o commit
git add .

# envia o commit da stage index ao repositorio (local) 
git commit -m "atualiza footer"

# mostra a lista de commits em uma linha cada 
git log --oneline --graph --all
# verifique que a tag e as branchs
```

## Merge


#### Fast-forward merge - juntar as modificações do footer com o main

```console
# mostra a lista de commits em uma linha cada 
git log --oneline --graph --all
# verifique que a tag e as branchs

# alternar para branch main
git checkout main

#combinar o footer com o main
git merge footer
# Apresenta Fast-forward merge na mensagem porque o main é parte da banch footer 
# -- * -- * -- * -- *
#   main          footer
# -- * -- * -- * -- *
#              (footer/main)

# mostra a lista de commits em uma linha cada 
git log --oneline --graph --all
# verifique que a tag e as branchs
```

#### Regular merge merge - juntar as modificações do sidebar com o main

```console
# mostra a lista de commits em uma linha cada 
git log --oneline --graph --all
# verifique que a tag e as branchs

# alternar para branch main
git checkout main

#combinar o footer com o main
git merge sidebar
# Apresenta Merge made by 'recursive' strategy
# -- * -- * -- * -- * main
#         |         
#         * -- * -- * sidebar

# -- * -- * -- * -- * -- * main
#         |              |
#         * -- * -- * ---
#                 sidebar


# mostra a lista de commits em uma linha cada 
git log --oneline --graph --all
# verifique que a tag e as branchs
```

## Forçar erro no merge

Crie um branch header-update e altere o header do index.html

```console
# mostra a lista de commits em uma linha cada 
git log --oneline --graph --all
# verifique que a tag e as branchs

# Cria a branch header-update e alterna para header-update
git checkout -b header-update

# mostra a lista de commits em uma linha cada 
git log --oneline --graph --all
# verifique que a tag e as branchs
```

Altere o index.html no header
```html
<header>
  <h1>Paralamas do Sucesso<h1>
</header>
```

```console
# adiciona todos os arquivos modificados no stage index para preparar o commit
git add .

# envia o commit da stage index ao repositorio (local) 
git commit -m "atualiza header"

# mostra a lista de commits em uma linha cada 
git log --oneline --graph --all
# verifique que a tag e as branchs
```

Retorne a branch main e altere o header do index.html

```console
# Alterna para main
git checkout main
```

Altere o index.html no header
```html
<header>
  <h1>Banda Eva<h1>
</header>
```

```console
# adiciona todos os arquivos modificados no stage index para preparar o commit
git add .

# envia o commit da stage index ao repositorio (local) 
git commit -m "atualiza main"

# mostra a lista de commits em uma linha cada 
git log --oneline --graph --all
# verifique que a tag e as branchs

```

Aplicando o Merge:
```console
# adiciona todos os arquivos modificados no stage index para preparar o commit
git merge header-update
# CONFLICT (content) ... 
# Automatic merge failed ...

# verifica a situação do seu repositorio
git status
# vão ser apresentados os arquivo onde há conflitos a serem corrigidos
```

Edite o arquivo index.html:
```html
<body>
    <header>
<<<<<<< HEAD
        <h1>Banda Eva<h1>
=======
        <h1>Paralamas do Sucesso<h1>
>>>>>>> header-update
</header>
```
É preciso apagar todos indicadores do merge e o conteudo que voce não deseja que continue

<<<<<<< HEAD
=======
>>>>>>> header-update


```console

# adciona e envia o commit nesse caso não pede a mensagem já existe a mensagem do merge
# equivale a 
# git add .
# git commit
git commit -a

# mostra a lista de commits em uma linha cada 
git log --oneline --graph --all
# verifique que a tag e as branchs

```


# Repositórios
- REPO BLOG: [https://github.com/mariojp/course-git-blog-project](https://github.com/mariojp/course-git-blog-project)


# MATERIAL
