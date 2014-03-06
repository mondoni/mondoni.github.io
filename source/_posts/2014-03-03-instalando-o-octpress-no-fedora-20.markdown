---
layout: post
title: "Instalando o Octopress no Fedora 20"
date: 2014-03-03 15:15:10 -0300
comments: false
categories: [octopress, fedora]
---
{% img /images/octopress2.jpg octopress %}
<!-- more -->

Recentemente um amigo queria um sistema para criar sua própria _Base de Conhecimento_ e estava optando por utilizar o projeto __[MediaWiki](http://www.mediawiki.org/)__. Porém, em minha opinião, ao utilizar o projeto MediaWiki para uma Base de Conhecimento pessoal têm algumas desvantagens, tais como:

* Banco de Dados (NoSQL, MySQL, Postgres, etc);
* Servidor Web (Apache, Nginx, etc);
* Manutenção;
* Entre outras.

Então eu acabei recomendando o projeto __*Octopress*__ com base em alguns testes que efetuei alguns anos atrás, retornando a utilizá-lo para criar esta página de apresentação e howto de instalação.

## O que é o Octopress

Octopress é um sistema de blog estático que utiliza projeto [Jekyll](http://jekyllrb.com/) para gerar as páginas e efetuar a publicação de seu conteúdo.

Todo conteúdo a ser publicado com o _Octopress_ deve ser gerenciado por arquivos texto utilizando a sintaxe __*Markdown*__. Markdown utiliza uma sintaxe simples e amplamente conhecida por quem utiliza o site [Github](http://github.com) para hospedar seus projetos e efetuar a gerência de versão dos mesmos.

Você pode encontrar um excelente guia sobre _Markdown_ nesse link: [Mastering Markdown](http://guides.github.com/overviews/mastering-markdown/)

Algumas vantagens do _Octopress_ consiste em: 

* Não utilizar banco de dados para armazenar as informações dos posts;
* Os posts serem simples arquivos textos em Markdown;
* Integração com o __*Github Pages*__;
* __Ser extremamente simples de implementar e utilizar__.


## Instalação

#### Ambiente de Instalação

* __OS__ - _Fedora 20 (Heisenbug)_
* __Local__ - _Diretório **home** do usuário_

Obs.: para o funcionamento adequado do _Octopress_ deve ser utilizado _Ruby 1.9.3_ ou superior

#### Preparando o Ambiente

```bash
$ sudo yum install ruby rubygem-RedCloth ruby-devel rubygem-therubyracer gcc-c++
```

#### Instalando o Octopress

```bash
$ cd ~/
$ git clone git://github.com/imathis/octopress.git octopress
$ cd octopress/
```

Na versão utilizada nessa instalação (2.5) deve ser efetuado uma alteração no arquivo `Gemfile`, adicionando as linhas:

```ruby
gem 'json'
gem 'execjs'
gem 'therubyracer'
```

Resultando no arquivo abaixo:

```ruby
source "https://rubygems.org"

group :development do
  gem 'rake', '~> 0.9'
  gem 'jekyll', '~> 0.12'
  gem 'rdiscount', '~> 2.0.7'
  gem 'pygments.rb', '~> 0.3.4'
  gem 'RedCloth', '~> 4.2.9'
  gem 'haml', '~> 3.1.7'
  gem 'compass', '~> 0.12.2'
  gem 'sass', '~> 3.2'
  gem 'sass-globbing', '~> 1.0.0'
  gem 'rubypants', '~> 0.2.0'
  gem 'rb-fsevent', '~> 0.9'
  gem 'stringex', '~> 1.4.0'
  gem 'liquid', '~> 2.3.0'
  gem 'directory_watcher', '1.4.1'
  gem 'json'
  gem 'execjs'
  gem 'therubyracer'
end

gem 'sinatra', '~> 1.4.2'
```

O próximo passo é instalar o pacote *bundler*:

```bash
$ gem install bundler
$ bundle install 
```
Caso ocorra o erro abaixo na instalação do bundle, você não deve ter o pacote __*ruby-devel*__ instalado.

```bash
Gem::Installer::ExtensionBuildError: ERROR: Failed to build gem native extension.
    /usr/bin/ruby extconf.rb 
mkmf.rb can't find header files for ruby at /usr/share/include/ruby.h
```

E o passo final da instalação é referente ao tema padrão utilizado pelo _Octopress_:

```bash
$ rake install
## Copying classic theme into ./source and ./sass
mkdir -p source
cp -r .themes/classic/source/. source
mkdir -p sass
cp -r .themes/classic/sass/. sass
mkdir -p source/_posts
mkdir -p public
```

## Configurando o Octopress

A configuração do _Octopress_ é de extrema facilidade, o principal arquivo a ser alterado é `_config.yml` e as principais variáveis são:

```yaml
url:		 # For rewriting urls for RSS, etc
title:		# Utilizado no cabeçalho e tags título
subtitle:	# Uma descrição utilizada no cabeçalho
author:		# Seu nome, para RSS, Copyright, Metadata 
```

Abaixo está a configuração do meu _Octopress_:

```
url: http://mondoni.github.io
title: The Endless Search for Knowledge
subtitle:
author: 
simple_search: http://google.com/search
description:
```

Obs.: eu optei por manter a variável `author` em branco, pois eu sou o único a fazer uso desse Blog.

O Octopress por padrão traz algumas configurações do próprio _Jekyll_ e alguns plugins adicionais que será comentado mais adiante no post.

## Efetuando o Deploy do Octopress

É possível efetuar o deploy do Octopress das seguintes formas:

#### Github Pages

Serviço oferecido gratuitamente pelo Github que possibilita a criarde uma página pessoal com base num repositório git nomeado no padrão `username.github.io`. Também possibilita configurar um domínio próprio, caso você possua um.

#### Heroku

Serviço gratuíto semelhante ao Github Pages que também possibilita criar uma página pessoal com base num repositório git.

#### Rsync

Opção para caso você possua seu próprio servidor Web, assim podendo efetuar a sincronização do seu Octopress utilizando __rsync__ e __SSH__.

#### Configurando o Deploy com Github Pages

Como eu achei mais prático fazer uso do Github, irei demonstrar como configurar o deploy com __*[Github Pages](http://pages.github.com/)*__. Porém caso gostaria de maiores informações sobre os outros métodos, você encontrará no guia oficial sobre deploying: [Deploying Octopress](http://octopress.org/docs/deploying/).

Obviamente você deve ter uma conta no Github, então caso ainda não possua [clique aqui](https://github.com/join) para criar a sua conta.

Tendo sua conta em "mãos", crie um novo repositório cujo nome deve estar no seguinte formato `username.github.io`, onde _username_ é o nome do seu usuário criado no Github.

O Github Pages usa o branch _master_ como diretório público para seu webserver provendo o acesso a sua página no endereço `username.github.io`. Todo conteúdo a ser criado no Octopress deverá ser dentro do diretório __*source*__, que também será um branch de mesmo nome do repositório criado no Github.

O Octopress tem sua própria ferramenta para configurar a utilização do Github Pages, sem a necessidade de seguir a documentação do próprio Github. Para iniciar a configuração execute o comando abaixo:

```bash
$ rake setup_github_pages
```

O comando irá requisitar a URL SSH do seu repositório (ex.: `git@github.com:username/username.github.io.git`), o restante dos passos serão automáticos.

O próximo passo será gerar o seu blog:

```bash
$ rake generate 
$ rake deploy
```

O passo acima irá gerar o seu blog, copiar os arquivos gerados para dentro do diretório `_deploy/`, adicioná-los ao git, efetuar o commit e enviar os arquivos para o branch master automaticamente.

Caso você queira ver um preview do seu blog antes de efetuar o deploy, execute:

```bash
$ rake preview
```

Isso irá iniciar um webserver local, e você poderá acessar o preview do seu blog no endereço: http://localhost:4000

Como todo conteúdo é criado dentro do diretório `source/`, você não deve esquecer de efetuar o commit do branch source também. Sendo o primeiro commit após gerar o blog, execute os comandos abaixo:

```bash
$ git add .
$ git commit -m 'sua mensagem'
$ git push origin source
```

## Criando Conteúdo no Octopress

O Octopress traz consigo algumas ferramentas que auxilia na criação dos posts e páginas para o seu blog, como também irá automaticamente criar arquivos _XML_ para utilizar como feeds de notícia. Será gerado um arquivo geral (`atom.xml`) e um baseado em suas categorias (`blog/categories/<category>/atom.xml`).

#### Posts

Os posts devem ser armazenados dentro do diretório `source/_posts` e nomeados de acordo com o padrão do Jekyll `YYYY-MM-DD-post-title.markdown`. Com a ferramenta provida pelo Octopress o arquivo é criado da forma correta automaticamente, para criar o seu post execute o comando abaixo:

```bash
$ rake new_post["title"]
```

Exemplo:

```bash
$ rake new_post["Instalando o Octpress no Fedora 20"]
```

E o arquivo `source/_posts/2014-03-03-instalando-o-octpress-no-fedora-20.markdown` é criado e irá encontrar o conteúdo:

```yaml
---
layout: post
title: "Instalando o Octpress no Fedora 20"
date: 2014-03-03 15:15:10 -0300
comments: false
categories:
---
```

Nessa seção você pode habilitar ou não comentários, adicionar categorias, especificar o autor do post com `author: Nome Autor`. Caso esteja escrevendo um rascunho e opte por não publicar ao gerar o blog você pode adicionar a opção `published: false`.

Na opção categorias você pode usar uma única categoria ou múltiplas:

```
# Uma categoria
categories: octopress

# Múltiplas categorias - exemplo 1
categories: [octopress, fedora, linux]

# Múltiplas categorias - examplo 2
categories:
- octopress
- fedora
- linux 
```

O conteúdo do post em si deve ser adicionado abaixo do final da seção indicado pelos três traços: `---`

#### Páginas

Novas páginas podem ser adicionadas em qualquer lugar dentro do diretório `source` e o Jekyll irá reconhecer e gerar a mesma para publicação. Mas o Octopress também uma ferramenta que facilita a criação de novas páginas:

```bash
$ rake new_page[super-awesome]
```

E a página `/source/super-awesome/index.markdown` será criada com um conteúdo semelhante ao abaixo:

```yaml
---
layout: page
title: "Super Awesome"
date: 2014-03-03 5:59
comments: true
sharing: true
footer: true
--- 
```

#### Conteúdo

Adicionalmente como opção para os posts você pode optar por adicionar o comentário `<!-- more -->` para que todo conteúdo abaixo dele não seja exibido ná página principal que contêm todos os posts, resultando na exibição de um botão escrito "Read On ->".

## Gerando o Blog

E para finalizar e poder publicar o seu blog com os posts criados, você deve executar os comandos para gerar e publicar o mesmo.

#### Gerando

Para gerar o blog, execute:

```bash
$ rake generate
```

#### Publicando

Para publicar o blog no Github Pages enviando os arquivos para o repositório remoto, execute:

```bash
$ rake deploy
```

Lembrando de também de atualizar o branch source separadamente, como comentado na seção referente ao Github Pages.

#### Preview

Lembrando que existe a opção de preview local do seu blog pelo endereço: `http://localhost:4000`, execute:

```bash
$ rake preview
```

## Finalizando

Aqui finalizo o howto de instalação e configuração, bem como o básico de publicação.

Aguarde novos posts explicando o uso de plugins, themes, etc.

Cheers!!
