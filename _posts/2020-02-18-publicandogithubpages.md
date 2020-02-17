---
layout: post
title: Publicando com o GitHub Pages 
subtitle: Uma forma simples de publicar um site utilizando o Github Pages. 
tags: [ghpages,github,githubpages,pages]
gh-repo: mariojp/page-sample
gh-badge: [githubpage]
comments: true
---

# Parar publicar um site ou documentação com o GitHub Pages só precisa de 3 etapas:

1. Crie um novo repositório ou use um repositório existente
2. Envie um arquivo de Markdown através da interface da web ou pelo git commit.
3. Ative as Páginas do GitHub através das configurações do seu repositório.

## Usando HTML

1. Clique em Novo Repositorio.

2. Configure o repositorio. 
  + Escolha um nome
    + Se nome do repositorio for nomedousuario.github.io vai ser gerado um endereço http://nomedousuario.github.io
    + Se for qualque outro nome do repositorio vai ser gerado um endereço http://nomedousuario.github.io/nomedorepositorio
  + Marque para inicializar o repositorio.

3. Crie um novo arquivo chamado index.html com o seguinte conteudo.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello World! Site Title</title>
  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

3. Clique em  Settings

4. Procure por GitHub Pages
  + Opção Source
    + Selecione master branch pe o index estiver na raiz caso o nome do seu repositorio nomedousuario.github.io essa opção é obrigatoria
    
![GitHub Pages](/githubpages.png)

## Usando o Jekill

1. Clique em Novo Repositorio.

2. Configure o repositorio. 
  + Escolha um nome
    + Se nome do repositorio for nomedousuario.github.io vai ser gerado um endereço http://nomedousuario.github.io
    + Se for qualque outro nome do repositorio vai ser gerado um endereço http://nomedousuario.github.io/nomedorepositorio
  + Marque para inicializar o repositorio.

3. Clique em  Settings

4. Para ativar clique em Settings e ative as Páginas do GitHub.
  + Altere a opção Source do Github Pages para: master branch
  + Escolha um tema na opção Theme chooser.
  + Ao salvar o github vai apresentar o link para sua pagina.

Existem outra opção para publicação como escolher no Source: master branch /docs folder onde voce deve criar uma pasta docs que será a raiz da sua pagina.

Por baixo dos panos para que tudo isso funcione o os arquivos de Markdown são renderizados pelas Páginas GitHub e o arquivo README.md é utilizado como o índice do site se você não tiver um index.md (ou index.html).

Se não houver tema escolhido é definido um tema padrão mínimo que corresponda à aparência do Markdown padrão no GitHub. Com a escolha de um tema o Github Pages adiciona um arquivo _config.yml que contem a configuração do tema.

Para quem quiser ir mais alem boa parte da magica é feita com uso do Jekyll (https://jekyllrb.com).

“Para que o processo de criação das páginas do GitHub possa ser o mais transparente e personalizável possível, todos os recursos acima são implementados como plugins Jekyll de código aberto, como Jekyll Opcional Front Matter, Jekyll README Index, Jekyll Padrão Layout e Jekyll Títulos de Cabeçalhos”. (Traduzido da documentação)

Não perca tempo e teste agora ou coloque documentação no seu site.
